# Minecraft Server Migration and Hosting

## Overview

A previously configured vanilla Minecraft Java Edition server was migrated from a Windows laptop to a dedicated Debian 13 LXC container running on Proxmox VE.

The goal was to preserve the existing survival world, player inventories, whitelist, operator privileges, server settings, and historical world backup while moving the workload to always-on homelab infrastructure.

This project provided hands-on experience with server migration, Linux containers, SSH/SCP file transfer, Java runtime management, file permissions, systemd services, DHCP and subnet troubleshooting, port forwarding through two routers, and Proxmox backups.

## Architecture

The current hosting and network path is:

    Internet / Remote Players
              |
              | TCP 25565
              |
        AT&T Gateway
        192.168.1.254
              |
              | Port forward to ER605 WAN
              |
        TP-Link ER605
        WAN: 192.168.1.143
        LAN: 192.168.0.1
              |
              | Port forward to Minecraft LXC
              |
      Cisco Catalyst Switch
              |
        Proxmox Host (pve1)
        192.168.0.50
              |
        Debian 13 LXC (102)
        192.168.0.104
              |
       Minecraft Java 26.2
         TCP port 25565

The public IP address and administration credentials are intentionally excluded from this repository.

## LXC Container

Minecraft runs in its own Debian 13 LXC container on the existing Proxmox host, separate from Jellyfin and Plex.

Initial container configuration:

- Container ID: `102`
- Hostname: `minecraft`
- Debian GNU/Linux 13
- 2 virtual CPU cores
- 4 GB RAM
- 1 GB swap
- 24 GB virtual disk
- Bridge: `vmbr0`
- DHCP-assigned IPv4 address: `192.168.0.104`
- Unprivileged container
- Container start at boot enabled

Using an LXC container provides a lightweight dedicated environment without requiring a full virtual machine.

The container's DHCP address still needs a reservation to prevent its IP from changing after a lease renewal.

## Migration from Windows

The original Minecraft server was hosted on a Windows laptop at:

`C:\MinecraftServer`

The existing server contained:

- `world/` — current survival world and player data
- `world-old/` — an earlier world retained for recovery
- `server.jar`
- `server.properties`
- `whitelist.json`
- `ops.json`
- `banned-players.json` and `banned-ips.json`
- `libraries/` and `versions/`
- `eula.txt`, logs, and supporting files

The original server was stopped before copying to prevent an inconsistent world backup.

A ZIP backup was created on Windows and moved through the main desktop, HUNTERMAIN, because the laptop was connected to the AT&T Wi-Fi subnet while the Proxmox host was behind the ER605 on a different subnet.

The backup was transferred to the Minecraft LXC using SCP and a dedicated `mc-transfer` account:

```powershell
scp "$env:USERPROFILE\Downloads\MinecraftServer-Backup.zip" mc-transfer@192.168.0.104:/home/mc-transfer/upload/
```

Inside the container, the archive was extracted and the server files were copied to their permanent location:

```bash
unzip -q /home/mc-transfer/upload/MinecraftServer-Backup.zip -d /home/mc-transfer/upload/extracted
cp -a /home/mc-transfer/upload/extracted/MinecraftServer/. /opt/minecraft/
```

The original Windows server and backup were preserved during migration.

## Java Runtime

The container initially had OpenJDK 21 installed. The migrated Minecraft 26.2 server required Java 25.

OpenJDK 25 was installed using Debian's package repositories:

```bash
apt install openjdk-25-jre-headless -y
java -version
```

The Java alternative was checked using:

```bash
update-alternatives --config java
```

Java 25 became the default runtime.

## Linux User and Permissions

A dedicated system account was created so the Minecraft process would not run as root:

```bash
useradd -r -m -d /opt/minecraft -s /usr/sbin/nologin minecraft
```

The first server startup failed while extracting libraries, with an `AccessDeniedException` for `/opt/minecraft/versions/26.2`.

Inspection with `ls -ld` showed that some directories extracted from the Windows ZIP lacked the execute (`x`) permission required to traverse Linux directories.

The issue was resolved using:

```bash
chown -R minecraft:minecraft /opt/minecraft
find /opt/minecraft -type d -exec chmod 755 {} +
find /opt/minecraft -type f -exec chmod 644 {} +
```

This exercise reinforced the difference between ownership and permission bits, and the importance of directory execute permission on Linux.

## Minecraft Service Management

Minecraft was initially tested by launching it interactively:

```bash
cd /opt/minecraft
runuser -u minecraft -- java -Xms1G -Xmx4G -jar server.jar nogui
```

After confirming that the existing world loaded and a local player could join, a systemd service was created at:

`/etc/systemd/system/minecraft.service`

Service configuration:

```ini
[Unit]
Description=Minecraft Java Server
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=minecraft
Group=minecraft
WorkingDirectory=/opt/minecraft
ExecStart=/usr/bin/java -Xms1G -Xmx4G -jar /opt/minecraft/server.jar nogui
ExecStop=/bin/kill -s SIGTERM $MAINPID
Restart=on-failure
RestartSec=10
TimeoutStopSec=120

[Install]
WantedBy=multi-user.target
```

The service was enabled and started using:

```bash
systemctl daemon-reload
systemctl enable --now minecraft
systemctl status minecraft
```

Logs were inspected with:

```bash
journalctl -u minecraft -n 30 --no-pager
```

The logs confirmed Minecraft 26.2 completed startup and saved the Overworld, Nether, and End. The service was also observed running after a Proxmox backup operation.

Useful administration commands:

```bash
systemctl status minecraft
systemctl restart minecraft
systemctl stop minecraft
journalctl -u minecraft -f
```

## Network Configuration and Troubleshooting

The homelab uses two separate IPv4 subnets:

- AT&T gateway and Wi-Fi clients: `192.168.1.0/24`
- ER605 LAN, Cisco switch, and Proxmox services: `192.168.0.0/24`

The initial SCP attempt from the laptop to the LXC timed out even though SSH was active and listening on port 22.

Troubleshooting included:

- `systemctl status ssh` — confirmed OpenSSH was running
- `ip -4 addr show eth0` — confirmed LXC DHCP address
- `ip route` — confirmed default gateway
- `ping` and `tracert` — investigated connectivity
- `pct exec` — ran commands inside the container from the Proxmox host
- `ip neigh show` — inspected local neighbor entries

The Proxmox host could reach the LXC, but the laptop on the AT&T Wi-Fi subnet could not reach the homelab subnet. The transfer was completed through HUNTERMAIN after switching its network connection as needed.

This demonstrated the difference between a working local bridge/network interface and end-to-end reachability across routed networks.

## Port Forwarding and External Access

The AT&T gateway and TP-Link ER605 both perform routing/NAT.

The external Minecraft traffic path is:

    Internet
       |
       | TCP 25565
       v
    AT&T Gateway
       |
       | Forward TCP 25565 to ER605 WAN
       v
    ER605 WAN (192.168.1.143)
       |
       | Forward TCP 25565
       v
    Minecraft LXC (192.168.0.104:25565)

On the ER605, the forwarding rule was created under:

`Transmission -> NAT -> Virtual Servers`

Rule:

- Name: `Minecraft`
- Interface: `WAN`
- External port: `25565`
- Internal IP: `192.168.0.104`
- Internal port: `25565`
- Protocol: `TCP`
- Status: enabled

On the AT&T gateway, the existing Minecraft NAT/Gaming rule previously targeting the Windows laptop was replaced with a rule targeting the ER605.

An external TCP port check succeeded, showing the service was reachable on port 25565. Full external Minecraft login by a friend has not yet been tested.

The server retains its existing whitelist and operator configuration. Management ports such as Proxmox and SSH are not intentionally exposed by these Minecraft forwarding rules.

## Backups

The original Windows server was archived before migration, including the current world and `world-old`.

A Proxmox backup job was then configured for Minecraft container 102:

- Storage: `local`
- Schedule: daily around 4:00 AM
- Backup mode: `Stop`
- Compression: ZSTD
- Retention: last 3 backups

A manual backup test was completed, and the Minecraft service was observed running afterward.

Backups currently stored on the Proxmox host do not protect against total failure of the host SSD. A restore test to a separate container ID and off-host backup storage remain future improvements.

## Current Result

The Windows-hosted Minecraft server has been successfully migrated to a Debian 13 LXC container.

Completed and verified:

- Preserved existing Minecraft server files and world
- Created dedicated Proxmox LXC
- Installed Java 25
- Resolved Linux ownership and permission errors
- Launched migrated Minecraft 26.2 world
- Connected successfully from HUNTERMAIN over the LAN
- Created and enabled a systemd service
- Verified successful startup through systemd logs
- Configured forwarding through AT&T and ER605
- Confirmed external TCP port 25565 reachability
- Configured Proxmox backups and tested the backup process

External player login remains to be tested.

## Future Improvements

Planned tasks include:

- Test a friend's external login on Minecraft 26.2
- Reserve the ER605 WAN address and Minecraft LXC LAN address in DHCP
- Verify root administrative access to the container
- Restore a backup to a separate test container to validate recovery
- Move at least one backup copy to separate physical storage
- Review exposed ports, firewall rules, and server whitelist security
- Evaluate performance, RAM usage, and player capacity
- Integrate Minecraft service logs with Splunk
- Analyze connection attempts and network traffic using Wireshark
- Create alerts for server crashes, login failures, and whitelist rejections
- Document maintenance, upgrades, and recovery procedures

## Skills Developed

This project provided hands-on experience with:

- Proxmox VE and LXC
- Debian Linux administration
- Windows-to-Linux workload migration
- SSH and SCP file transfers
- Linux file ownership and permissions
- Java runtime installation and version selection
- systemd service creation and management
- Linux log inspection using journalctl
- DHCP, routing, subnets, and default gateways
- NAT and TCP port forwarding through multiple routers
- Network connectivity troubleshooting
- Minecraft server operations and access controls
- Proxmox backup configuration and verification
- Infrastructure documentation
