\# Jellyfin Media Server



\## Overview



Jellyfin is deployed as a self-hosted media server inside a Debian 13 LXC container running on Proxmox VE.



This project provided hands-on experience with Linux containers, network storage, SMB/CIFS, mount points, permissions, service administration, and troubleshooting connectivity between Windows, Proxmox, and Linux.



\## Architecture



The current media path is:



Main Windows PC

&#x20;   |

&#x20;   | SMB/CIFS Share

&#x20;   |

Proxmox Host

&#x20;   |

&#x20;   | Bind Mount

&#x20;   |

Debian 13 LXC

&#x20;   |

Jellyfin

&#x20;   |

&#x20;   +-- Movies

&#x20;   +-- TV

&#x20;   +-- Music



The Windows workstation currently provides the physical storage for the media library. Proxmox accesses this storage over the network and makes it available to the Jellyfin container.



\## LXC Container



Jellyfin runs inside a Debian 13 LXC container on the Proxmox host.



Initial container resources:



\- 2 virtual CPU cores

\- 2 GB RAM

\- DHCP networking

\- Proxmox Linux bridge connectivity



Using an LXC container provides a lightweight environment for the service without requiring the resources of a complete virtual machine.



\## Jellyfin Installation



Jellyfin was installed inside the Debian container and configured as the primary media server.



The server is administered through Jellyfin's web interface from devices on the local network.



Libraries were created for:



\- Movies

\- Television

\- Music



Jellyfin scans these directories and retrieves metadata and artwork for the media library.



\## Network Storage



The media library is currently stored on the main Windows workstation rather than on the Proxmox host's internal SSD.



The Windows system exposes media directories using SMB/CIFS file sharing.



Current shared media directories include:



\- Movies

\- TV

\- Music

\- Downloads



The SMB share is mounted on the Proxmox host.



\## LXC Bind Mount



After mounting the SMB share on Proxmox, the media directory is passed into the Jellyfin LXC container using a bind mount.



Inside the container, the media is available at:



`/mnt/media`



Example directory structure:



/mnt/media/

&#x20;   Movies/

&#x20;   TV/

&#x20;   Music/

&#x20;   Downloads/



This allows Jellyfin to access network-hosted media without storing the media files inside the container's virtual disk.



\## Read-Only Media Access



The media mount presented to the Jellyfin container is configured as read-only.



This design allows Jellyfin to:



\- Scan media

\- Read video and audio files

\- Retrieve metadata

\- Stream content



while preventing the container from modifying or deleting the original media files.



This provides an additional layer of protection for the media library.



\## Storage Separation



The architecture separates application storage from media storage.



The Proxmox `local-lvm` storage contains the Jellyfin container's virtual disk and operating system.



The actual media library remains on network-attached storage provided by the Windows workstation.



This prevents large media files from consuming the limited internal SSD capacity of the Proxmox host.



\## Administration and Troubleshooting



During deployment, troubleshooting included verifying:



\- SMB connectivity

\- Proxmox mount availability

\- LXC bind mounts

\- Linux file permissions

\- Container network connectivity

\- Jellyfin library paths

\- Media scanning

\- Client connectivity



Proxmox container management tools were also used to inspect the container and verify that the mounted directories were accessible from inside the LXC environment.



\## Current Result



Jellyfin is operational on the local network and can:



\- Access the network-hosted media library

\- Scan and organize media

\- Retrieve metadata

\- Stream media to supported client devices



\## Future Improvements



Planned improvements include:



\- Dedicated media storage

\- Improved backup strategy

\- Secure remote access

\- Additional client configuration

\- Network segmentation

\- Monitoring

\- Migration away from workstation-hosted media storage as the homelab expands

