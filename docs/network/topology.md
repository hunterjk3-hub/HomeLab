&#x20;   # HomeLab Network Topology



\## Overview



This document describes the current network architecture of my personal homelab. The environment is designed to provide hands-on experience with networking, virtualization, Linux administration, self-hosted services, and infrastructure troubleshooting.



The network currently uses a TP-Link Omada ER605 router and a Cisco Catalyst 2960CG managed switch to provide connectivity to physical hosts and virtualized services.



\## Physical Topology



Internet

&#x20;  |

Fiber Gateway

&#x20;  |

TP-Link Omada ER605

&#x20;  |

Cisco Catalyst 2960CG-8TC-L

&#x20;  |

&#x20;  +-- Main Windows PC

&#x20;  |

&#x20;  +-- Lenovo ThinkCentre M900 Tiny

&#x20;         |

&#x20;         +-- Proxmox VE

&#x20;                |

&#x20;                +-- LXC Containers

&#x20;                |

&#x20;                +-- Virtual Machines



\## Network Components



\### Fiber Gateway



Provides the connection between the home network and the Internet.



\### TP-Link Omada ER605



The ER605 serves as the primary router for the homelab network.



Current responsibilities include:



\- Routing traffic between the local network and upstream gateway

\- Providing connectivity to the Cisco switch

\- Serving as the foundation for future VLAN and network segmentation projects



\### Cisco Catalyst 2960CG-8TC-L



The Cisco Catalyst switch provides wired Ethernet connectivity for homelab devices.



The switch is being used to gain hands-on experience with:



\- Managed Ethernet switching

\- Cisco IOS

\- Switch port configuration

\- Network troubleshooting

\- Future VLAN and trunk configuration



\### Proxmox Host



A Lenovo ThinkCentre M900 Tiny currently serves as the primary virtualization host.



Hardware:



\- Intel Core i5-6600

\- 16 GB RAM

\- 256 GB SSD



The system runs Proxmox VE and hosts virtualized services using LXC containers and virtual machines.



\### Main Windows PC



The primary Windows workstation is connected to the network and is also used for administration of the homelab.



Current uses include:



\- PowerShell administration

\- Accessing the Proxmox management interface

\- Network troubleshooting

\- Hosting shared storage used by some homelab services

\- Git and GitHub repository management



\## Virtual Networking



Proxmox uses a Linux bridge to provide network connectivity to virtual machines and LXC containers.



This allows virtualized services to communicate with devices on the physical LAN while remaining hosted on the Proxmox server.



\## Current Network Design



The environment currently operates primarily as a single LAN while the initial infrastructure and services are being deployed.



Future improvements will include network segmentation using VLANs.



Potential network segments include:



\- Management

\- Servers

\- Trusted clients

\- IoT devices

\- Guest devices



\## Planned Improvements



\- Configure VLANs

\- Configure Cisco switch access and trunk ports

\- Implement network segmentation

\- Deploy Pi-hole for DNS filtering

\- Deploy Tailscale for secure remote access

\- Add network monitoring

\- Document IP addressing

\- Create a graphical network topology diagram

