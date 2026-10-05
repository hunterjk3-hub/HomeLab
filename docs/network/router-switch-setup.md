\# Router and Switch Setup



\## Overview



The physical network infrastructure for the homelab consists of a TP-Link Omada ER605 router and a Cisco Catalyst 2960CG-8TC-L managed switch.



The goal of this setup is to provide a dedicated environment for learning routing, switching, Cisco IOS, network troubleshooting, and eventually VLAN-based network segmentation.



\## Physical Connections



The current physical network path is:



Internet / ISP

&#x20;     |

Fiber Gateway

&#x20;     |

TP-Link Omada ER605

&#x20;     |

Cisco Catalyst 2960CG-8TC-L

&#x20;     |

&#x20;     +-- Main Windows PC

&#x20;     |

&#x20;     +-- Lenovo ThinkCentre M900 Tiny

&#x20;            |

&#x20;            +-- Proxmox VE



The laptop used for some administration does not have a built-in Ethernet port and therefore is not directly connected to the Cisco switch.



\## TP-Link Omada ER605



The TP-Link Omada ER605 serves as the router for the homelab network.



It connects the existing Internet gateway to the lab's wired network and provides an environment for learning router configuration and network segmentation.



\### Current Role



\- Provides routing for the homelab

\- Connects the homelab to the upstream Internet gateway

\- Provides connectivity to the Cisco managed switch

\- Provides the foundation for future VLAN configuration



\## Cisco Catalyst 2960CG-8TC-L



A Cisco Catalyst 2960CG-8TC-L is used as the primary managed Ethernet switch.



The switch connects the physical systems in the lab, including:



\- Main Windows workstation

\- Proxmox virtualization host



The switch also provides a platform for gaining hands-on experience with Cisco IOS and managed network infrastructure.



\## Skills Being Developed



This setup is being used to develop practical experience with:



\- Ethernet networking

\- Routing and switching concepts

\- Managed switches

\- Cisco IOS

\- Switch port configuration

\- IP addressing

\- Network troubleshooting

\- VLAN concepts

\- Network segmentation



\## Future Configuration



As the homelab expands, the network will be segmented using VLANs.



Planned network segments include:



\- Management

\- Servers

\- Trusted client devices

\- IoT devices

\- Guest devices



The Cisco switch and ER605 will be configured to support VLAN tagging and traffic separation between these networks.



Additional documentation will be added as these configurations are implemented.

