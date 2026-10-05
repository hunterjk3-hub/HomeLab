\# Proxmox Networking



\## Overview



Proxmox VE uses Linux bridge networking to connect virtual machines and LXC containers to the physical homelab network.



This allows virtualized services running on the Proxmox host to communicate with other devices on the LAN as though they were separate networked systems.



\## Physical Network Path



The Proxmox host is physically connected to the Cisco Catalyst switch using Ethernet.



Current path:



Internet / ISP

&#x20;     |

Fiber Gateway

&#x20;     |

TP-Link Omada ER605

&#x20;     |

Cisco Catalyst 2960CG-8TC-L

&#x20;     |

Lenovo ThinkCentre M900 Tiny

&#x20;     |

Proxmox VE

&#x20;     |

Linux Bridge

&#x20;     |

LXC Containers / Virtual Machines



\## Linux Bridge



Proxmox uses a Linux bridge to provide network connectivity to virtualized workloads.



The bridge functions similarly to a virtual Ethernet switch inside the Proxmox host.



The physical Ethernet interface connects the Proxmox server to the physical network, while virtual interfaces from containers and virtual machines connect to the Linux bridge.



Conceptually:



Physical NIC

&#x20;    |

&#x20;    |

Linux Bridge

&#x20; |      |

&#x20; |      +-- LXC Container

&#x20; |

&#x20; +--------- Virtual Machine



This allows the Proxmox host and its virtualized workloads to share the physical network connection while maintaining their own network interfaces.



\## Container Networking



LXC containers can be assigned virtual network interfaces connected to the Proxmox Linux bridge.



For the initial Jellyfin deployment, the container was configured to use DHCP.



This allowed the router's DHCP service to assign an IP address to the container automatically.



The Jellyfin container can then communicate with:



\- The Proxmox host

\- Other devices on the LAN

\- The Windows workstation hosting the media share

\- Client devices accessing Jellyfin

\- The Internet when required for updates and metadata



\## Network Troubleshooting



Working with Proxmox networking has involved troubleshooting connectivity between:



\- Physical hosts

\- LXC containers

\- Network storage

\- Client devices

\- Self-hosted applications



Tools and techniques used include:



\- ICMP/ping connectivity testing

\- IP address verification

\- Linux command-line networking tools

\- Proxmox container management commands

\- Service and port connectivity testing



\## Future VLAN Configuration



The current environment primarily operates on a single LAN.



A future project will introduce VLAN-based network segmentation using the TP-Link ER605 router and Cisco Catalyst managed switch.



The Proxmox bridge can then be configured to carry VLAN-tagged traffic to virtualized workloads.



Potential network segments include:



\- Management

\- Servers

\- Trusted clients

\- IoT

\- Guest devices



This will provide additional experience with VLAN tagging, trunk ports, routing between networks, firewall policies, and infrastructure security.

