\# Proxmox VE Installation



\## Overview



Proxmox VE is used as the primary virtualization platform for my homelab.



It is installed on a Lenovo ThinkCentre M900 Tiny and provides the platform for running Linux containers (LXC) and virtual machines used for self-hosted services and infrastructure projects.



\## Host Hardware



\### Lenovo ThinkCentre M900 Tiny



\- CPU: Intel Core i5-6600

\- Memory: 16 GB RAM

\- Storage: 256 GB SSD

\- Hypervisor: Proxmox VE



The small-form-factor system was selected as an inexpensive and power-efficient platform for learning virtualization and running lightweight homelab services.



\## Installation



Proxmox VE was installed directly onto the internal SSD as a bare-metal hypervisor.



During installation, the system was configured with:



\- ext4 filesystem

\- Local Proxmox management account

\- Network interface for management access

\- Proxmox Linux bridge for virtual networking



After installation, the Proxmox web interface was used for management and deployment of virtualized services.



\## Storage Layout



The initial installation created two primary Proxmox storage locations:



\### local



Directory-based storage used for items such as:



\- ISO images

\- Container templates

\- Backups



Approximately 71 GB is currently allocated to this storage.



\### local-lvm



LVM-thin storage used primarily for virtual machine and LXC container disks.



Approximately 148 GB is currently allocated to this storage.



Additional storage is planned as the homelab expands.



\## Container Strategy



LXC containers are being used for lightweight Linux services where a complete virtual machine is unnecessary.



The first major service deployed using this approach was a Debian-based Jellyfin media server.



Virtual machines can be used in the future for workloads that require stronger isolation or a complete guest operating system.



\## Administration



The Proxmox host can be administered through:



\- Proxmox web interface

\- Linux command line

\- SSH/remote administration

\- Proxmox container management tools



Working with the host has provided hands-on experience with Linux administration, virtualization, storage management, networking, and troubleshooting.



\## Current Workloads



Current virtualized services include:



\- Jellyfin media server running in a Debian LXC container



Additional services are planned as the environment expands.



\## Planned Improvements



\- Deploy additional infrastructure services

\- Configure Tailscale remote access

\- Deploy Pi-hole

\- Expand storage

\- Implement VLAN-aware networking

\- Add monitoring

\- Evaluate an additional Proxmox node

