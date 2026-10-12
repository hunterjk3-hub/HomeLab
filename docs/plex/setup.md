

\# Plex Media Server



\## Overview



Plex Media Server is deployed as a self-hosted media streaming service inside a Debian LXC container running on Proxmox VE.



This project provided hands-on experience with Linux container deployment, package management, network storage, SMB/CIFS file sharing, bind mounts, Linux permissions, and application administration.



Plex was deployed alongside Jellyfin to gain experience managing multiple media server applications using shared infrastructure.



\## Architecture



The current media storage path is:



&#x20;   Main Windows PC

&#x20;       |

&#x20;       | SMB/CIFS Share

&#x20;       |

&#x20;   Proxmox Host

&#x20;       |

&#x20;       | Bind Mount

&#x20;       |

&#x20;   Debian LXC

&#x20;       |

&#x20;   Plex Media Server

&#x20;       |

&#x20;       +-- Movies

&#x20;       +-- TV

&#x20;       +-- Music



The Windows workstation provides the physical storage for the media library.



Proxmox accesses the shared storage over the network and exposes it to the Plex container through a bind mount.



This allows Plex and Jellyfin to access the same underlying media files without maintaining duplicate libraries.



\## LXC Container



Plex runs inside a Debian LXC container on the Proxmox host.



Initial container resources:



\- 2 virtual CPU cores

\- 512 MB RAM

\- 512 MB swap

\- Proxmox Linux bridge networking



The container provides a lightweight Linux environment for running Plex without requiring a complete virtual machine.



Using LXC also allows resources to be allocated independently between Plex and other services hosted on Proxmox.



\## Plex Installation



Plex Media Server was installed inside the Debian container using Linux package management tools.



Installation and configuration included:



\- Preparing the Debian container

\- Configuring package repository access

\- Adding the Plex repository signing key

\- Installing the Plex Media Server package

\- Managing the Plex service

\- Accessing the application through its web interface



This provided experience with third-party Linux software repositories and package management.



\## Network Configuration



The Plex container connects to the homelab network through the Proxmox Linux bridge.



This allows the service to communicate with:



\- The Proxmox host

\- The Windows workstation hosting media files

\- Other devices on the local network

\- Client devices accessing Plex

\- External services required for application functionality



Plex Media Server commonly uses TCP port `32400` for its web interface and streaming connections.



The Proxmox virtual networking environment allows the container to operate as a separate networked system while sharing the physical host's Ethernet connection.



\## Network Storage



The media library is stored on the main Windows workstation rather than the Proxmox host's internal SSD.



The Windows system exposes the media directories using SMB/CIFS file sharing.



Shared directories include:



\- Movies

\- TV

\- Music

\- Downloads



The SMB share is mounted on the Proxmox host and made available to the Plex container.



This allows the media server to access files stored on another physical computer across the local network.



\## LXC Bind Mount



After mounting the SMB share on the Proxmox host, the shared media directory was passed into the Plex LXC container using a bind mount.



Inside the container, the media is accessible at:



`/mnt/media`



The directory structure includes:



&#x20;   /mnt/media/

&#x20;       Movies/

&#x20;       TV/

&#x20;       Music/

&#x20;       Downloads/



The bind mount provides access to the shared files without copying them into the container's virtual disk.



This approach also allows multiple containers to access the same underlying media storage.



\## Storage Separation



The Plex deployment separates application storage from media storage.



The Proxmox `local-lvm` storage pool provides storage for the container's operating system and application files.



The media library remains on the Windows workstation and is accessed over the network.



This design provides several advantages:



\- Reduces internal SSD storage consumption

\- Avoids duplicate media files

\- Separates application data from media data

\- Allows independent management of the media library

\- Supports future migration to dedicated network storage



Both Plex and Jellyfin can use the same physical media library while maintaining their own application configurations and metadata.



\## Administration and Troubleshooting



During deployment, administration and troubleshooting included:



\- Debian package repository configuration

\- Repository signing key installation

\- Linux service management

\- Container resource allocation

\- Proxmox container administration

\- SMB share connectivity

\- LXC bind mount configuration

\- Linux filesystem permissions

\- Verification of mounted media directories



Proxmox command-line tools were used to inspect the container and verify access to the mounted storage.



These tasks provided experience troubleshooting issues across Linux, Windows, virtualization, and network storage environments.



\## Current Result



Plex Media Server has been deployed inside a Debian LXC container on the Proxmox host.



Completed configuration includes:



\- Debian LXC deployment

\- Plex package repository configuration

\- Plex Media Server installation

\- Container resource allocation

\- Proxmox bridge networking

\- SMB-backed media storage

\- LXC bind mount configuration

\- Verification of media directory accessibility



The project demonstrates the deployment of a second self-hosted application using the existing Proxmox and network storage infrastructure.



\## Future Improvements



Planned improvements include:



\- Dedicated media storage

\- Improved backup and recovery procedures

\- Secure remote access

\- Additional client configuration

\- Media server performance monitoring

\- CPU and memory utilization analysis

\- Network segmentation

\- Centralized logging

\- Migration away from workstation-hosted media storage



\## Skills Developed



This project provided hands-on experience with:



\- Proxmox VE

\- Linux container administration

\- Debian Linux

\- Linux package management

\- Third-party software repositories

\- Linux service management

\- SMB/CIFS networking

\- Network-mounted storage

\- LXC bind mounts

\- Linux filesystem permissions

\- TCP/IP networking

\- Virtual networking

\- Infrastructure troubleshooting



