\# Proxmox Storage Configuration



\## Overview



The Proxmox VE host currently uses a single 256 GB SSD for the operating system, container storage, virtual machine storage, templates, and other local Proxmox data.



The installation uses two primary storage pools:



\- `local`

\- `local-lvm`



\## Current Storage



Storage configuration reported by Proxmox:



| Storage | Type | Total | Used | Available |

|---|---|---:|---:|---:|

| local | Directory | \~67.7 GiB | \~5.4 GiB | \~58.8 GiB |

| local-lvm | LVM-Thin | \~141.2 GiB | \~3.9 GiB | \~137.3 GiB |



These values will change as additional containers, virtual machines, templates, and services are deployed.



\## local



`local` is directory-based storage managed by Proxmox.



It is primarily used for files such as:



\- Container templates

\- ISO images

\- Backups

\- Other file-based Proxmox data



The current `local` storage has approximately 67.7 GiB of total capacity.



\## local-lvm



`local-lvm` is an LVM-Thin storage pool.



It is primarily used for virtual disks belonging to:



\- LXC containers

\- Virtual machines



Thin provisioning allows storage to be allocated efficiently without immediately consuming the entire maximum virtual disk size.



The current `local-lvm` pool has approximately 141.2 GiB of total capacity.



\## Jellyfin Storage Design



The Jellyfin LXC container uses its Proxmox virtual disk for the operating system and application files.



Media files are not stored directly on the container's virtual disk.



Instead, media is stored on the main Windows workstation and shared across the network using SMB/CIFS. The share is mounted on the Proxmox host and exposed to the Jellyfin LXC container.



This separates application storage from the larger media library and avoids consuming the limited internal SSD capacity of the Proxmox host.



\## Future Storage Expansion



The current 256 GB SSD is sufficient for the initial homelab environment but is not intended to provide large-scale media storage.



Future plans include:



\- Dedicated mass storage

\- Additional hard drives

\- Improved backup strategy

\- Additional Proxmox storage

\- Possible redundant storage as the environment expands



Storage architecture and documentation will be updated as additional hardware is deployed.

