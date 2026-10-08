

\# Minecraft Java Server



\## Overview



A self-hosted Minecraft Java Edition server was deployed and configured on a Windows laptop to provide multiplayer access without relying on a third-party hosting provider.



This project provided hands-on experience with Java application hosting, Windows administration, TCP/IP networking, firewall configuration, port forwarding, access control, and troubleshooting remote connectivity.



The server currently operates on the Windows laptop, with plans to migrate it to the Proxmox virtualization environment.



\## Architecture



The current server network path is:



&#x20;   Internet

&#x20;       |

&#x20;   ISP Gateway / Router

&#x20;       |

&#x20;       | TCP Port Forwarding

&#x20;       |

&#x20;   Local Network

&#x20;       |

&#x20;   Windows Laptop (Wi-Fi)

&#x20;       |

&#x20;   Java Runtime Environment

&#x20;       |

&#x20;   Minecraft Server

&#x20;       |

&#x20;       +-- World Data

&#x20;       +-- Player Data

&#x20;       +-- Server Configuration

&#x20;       +-- Access Control



External players connect through the router, which forwards the designated Minecraft server port to the laptop hosting the server.



The server operates independently of commercial Minecraft hosting services, providing direct control over configuration, administration, and network accessibility.



\## Server Deployment



The Minecraft server was deployed on a Windows laptop using Java.



The installation process included:



\- Installing and configuring Java

\- Creating a dedicated server directory

\- Downloading the Minecraft Java server software

\- Configuring the server environment

\- Accepting the Minecraft EULA

\- Generating the initial world and configuration files

\- Starting and managing the server through the command line



Java provides the runtime environment required to execute the Minecraft server application.



\## Network Configuration



The server was configured to allow connections from both local and external players.



Minecraft Java Edition uses TCP port `25565` by default.



Network configuration involved:



\- Identifying the laptop's local IPv4 address

\- Configuring router port forwarding

\- Configuring Windows firewall rules

\- Allowing inbound TCP connections

\- Testing external network accessibility

\- Troubleshooting unsuccessful connection attempts



External connectivity testing confirmed that the server could be reached from outside the local network.



\## Port Forwarding



Port forwarding was configured to direct incoming Minecraft connections from the router to the Windows laptop.



The connection path follows:



&#x20;   External Minecraft Client

&#x20;       |

&#x20;   Internet

&#x20;       |

&#x20;   Router Public IP

&#x20;       |

&#x20;   TCP Port 25565

&#x20;       |

&#x20;   Port Forwarding Rule

&#x20;       |

&#x20;   Laptop Local IP

&#x20;       |

&#x20;   Minecraft Java Server



This configuration allows remote players to connect while the server remains hosted on the local network.



Port forwarding provided practical experience with Network Address Translation (NAT), TCP ports, internal IP addressing, and the security considerations of exposing services to the Internet.



\## Server Administration



The Minecraft server is administered using its command-line console and built-in management commands.



Administrative tasks include:



\- Starting and stopping the server

\- Managing server properties

\- Controlling player access

\- Assigning operator permissions

\- Managing the whitelist

\- Troubleshooting connection errors

\- Adjusting command feedback settings



The server console provides access to administrative commands and operational messages.



\## Whitelist Configuration



A whitelist was enabled to restrict access to approved Minecraft accounts.



Configuration involved:



\- Enabling whitelist enforcement

\- Adding authorized players

\- Managing the list of permitted accounts

\- Testing access restrictions

\- Troubleshooting rejected connections



Whitelisting provides application-level access control that limits which authenticated Minecraft accounts may join the server.



\## Operator Permissions



Operator permissions were configured to provide administrative capabilities to designated accounts.



These permissions allow authorized administrators to:



\- Execute administrative commands

\- Manage players

\- Modify server settings

\- Control gameplay administration

\- Perform server maintenance



This provided experience with permission management and separating standard users from administrators.



\## Server Data Management



The Minecraft server maintains several categories of files, including:



\- World files

\- Player data

\- Server configuration

\- Whitelist configuration

\- Operator permissions

\- Application logs



Understanding these files is important for server maintenance, troubleshooting, migration, and future backup automation.



The current world data remains stored on the Windows laptop.



\## Administration and Troubleshooting



During deployment and operation, troubleshooting included:



\- Java runtime configuration

\- Server startup and configuration issues

\- Router port forwarding

\- Windows firewall rules

\- Local and external connectivity

\- Player authentication errors

\- Invalid session errors

\- Whitelist access restrictions

\- Operator permission configuration

\- Command feedback settings



These tasks provided practical experience diagnosing problems across application, operating system, and network layers.



\## Current Result



The Minecraft Java server is currently hosted on a Windows laptop and supports multiplayer connections from authorized players.



Completed capabilities include:



\- Self-hosted Java server deployment

\- External network connectivity

\- Router port forwarding

\- Windows firewall configuration

\- Whitelist-based access control

\- Operator administration

\- Server configuration management

\- Remote connectivity troubleshooting



The project demonstrates hands-on experience deploying, configuring, administering, and troubleshooting a network-accessible application.



\## Future Improvements



Planned improvements include:



\- Migrate the Minecraft server from the Windows laptop to the Proxmox host

\- Preserve existing world data and player progress during migration

\- Configure automated world backups

\- Implement scheduled server maintenance

\- Monitor CPU, memory, and network utilization

\- Deploy centralized log collection using Splunk

\- Monitor rejected and unauthorized connection attempts

\- Configure automated alerts for server events

\- Analyze network traffic using Wireshark

\- Improve service availability and recovery procedures

\- Document the migration process and troubleshooting results



\## Skills Developed



This project provided hands-on experience with:



\- Windows administration

\- Java application hosting

\- TCP/IP networking

\- Network Address Translation (NAT)

\- Router port forwarding

\- Windows Defender Firewall

\- Remote connectivity troubleshooting

\- Access control and permissions

\- Command-line administration

\- File and configuration management

\- Server troubleshooting





