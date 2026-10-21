# Plex Remote Access and Troubleshooting

## Overview

This project documents configuring and troubleshooting remote access to a self-hosted Plex Media Server running in a Debian 13 LXC container on Proxmox VE.

The goal was to stream media to devices outside the home network, including family members' TVs, without exposing Proxmox administration, SMB storage, or unrelated homelab services to the internet.

This provided hands-on experience with TCP/IP networking, double NAT, port forwarding, router administration, Linux service verification, and end-to-end connectivity testing.

## Network Architecture

The network uses an AT&T gateway in front of a TP-Link ER605 router. The ER605 connects the homelab through a Cisco Catalyst switch and Proxmox Linux bridge.

```text
Internet
   |
AT&T Gateway
   | TCP 32400 forwarding
   |
TP-Link ER605 (WAN on AT&T private subnet)
   | TCP 32400 forwarding
   |
Cisco Catalyst Switch
   |
Proxmox Host (vmbr0)
   |
Debian 13 Plex LXC
   |
Plex Media Server (TCP 32400)
   |
SMB/CIFS-backed media storage
```

The ER605 receives a private WAN address from the AT&T gateway while Plex runs on the ER605's separate LAN subnet. This creates **double NAT**: inbound connections must be forwarded across both routing devices.

## Initial Problem

Plex was accessible from the local network, but **Settings > Remote Access** reported:

```text
Not available outside your network
```

The Plex server was signed into a Plex account, but the application reported that the server was not reachable from outside the network.

## AT&T Gateway Configuration

In the AT&T gateway's **Firewall > NAT/Gaming** interface, a custom service was created:

| Setting | Value |
| --- | --- |
| Service | Plex |
| Protocol | TCP |
| Global port range | 32400–32400 |
| Base host port | 32400 |
| Assigned device | TP-Link ER605 |

This forwards incoming Plex traffic from the AT&T gateway to the ER605's WAN interface.

The existing Minecraft forwarding rule was preserved.

## TP-Link ER605 Configuration

In **Transmission > NAT > Virtual Servers**, a separate rule was created:

| Setting | Value |
| --- | --- |
| Name | Plex |
| Interface | WAN |
| External port | 32400 |
| Internal port | 32400 |
| Internal server | Plex LXC private IP |
| Protocol | TCP |
| Status | Enabled |

This forwards the incoming TCP connection from the ER605 to the Plex container.

No DMZ or broad forwarding rule was used.

## Plex Configuration

Under **Plex > Settings > Remote Access**, the option **Manually specify public port** was enabled and set to:

```text
32400
```

Plex initially continued reporting that the server was not available outside the network, so the network path was investigated rather than opening additional ports.

## Troubleshooting

### Verify the Plex service

From the Plex LXC console, the following command was used:

```bash
ss -lntp | grep 32400
```

The output showed that Plex Media Server was listening on `0.0.0.0:32400`, confirming that the application was accepting IPv4 connections on the expected port.

### Verify Proxmox firewall configuration

The Plex container's firewall option was checked in Proxmox and found disabled. No firewall settings were changed.

### Verify both forwarding rules

The ER605 virtual-server rule was checked for the correct WAN interface, TCP protocol, external and internal port, destination container, and enabled status.

The AT&T gateway's Hosted Applications list confirmed that the Plex TCP 32400 service was assigned to the ER605.

### Test outside the local network

Wi-Fi was disabled on a phone so that the connection used cellular data.

The remote Plex client could see the server's libraries. Remote video playback through the Plex web interface also worked over cellular data.

**Result:** External media playback succeeded.

**Important limitation:** Playback success alone does not prove the connection was direct. Plex Relay can sometimes carry remote streams. A Plex Dashboard check for **Direct** versus **Indirect** during remote playback is still pending.

## Security Considerations

- Forwarded only the Plex TCP port `32400`.
- Did not forward Proxmox administration, SSH, or Windows SMB ports.
- Did not enable DMZ.
- Left unrelated Minecraft rules unchanged.
- Kept Plex Web's fallback to insecure connections set to **Never**.
- Limited internet exposure to the Plex service, while recognizing that any public-facing service must be maintained and patched.
- Avoided recording public IP addresses, credentials, access tokens, or sensitive router information in the public repository.

## Current Result

The Plex LXC serves media from an SMB/CIFS share mounted on the Proxmox host and passed into the container with an LXC bind mount.

Remote media playback was successfully tested using a phone on cellular data after configuring port forwarding through both the AT&T gateway and TP-Link ER605.

## Future Improvements

- Confirm direct remote playback rather than Plex Relay through the Plex Dashboard.
- Configure a DHCP reservation for the Plex LXC on the ER605.
- Stabilize the ER605 WAN address assignment on the AT&T gateway.
- Enable multi-factor authentication on the Plex account and maintain software updates.
- Set up individual Plex accounts and library sharing for family members.
- Test playback from family members' TVs.
- Monitor server CPU, bandwidth, and transcoding performance.

## Skills Developed

- Double NAT diagnosis
- TCP/IP networking
- Port forwarding and NAT configuration
- Router and gateway administration
- Linux service and socket inspection
- Proxmox LXC networking
- Firewall configuration verification
- Remote connectivity troubleshooting
- Network security fundamentals
- Technical documentation
