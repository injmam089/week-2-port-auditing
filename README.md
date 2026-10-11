# Week 2 - TCP/UDP Port Audit & Attack Surface Assessment



## Internship

GetIntern.in - 8-Week Practical Cybersecurity Track



## Objective



Perform an authorized TCP and UDP port audit of the local network,

identify exposed services, perform service fingerprinting, and create

an attack-surface risk matrix with security recommendations.



## Authorized Target



Local network:

192.168.1.0/24



All scanning activities were performed only against the authorized

local network for cybersecurity learning and internship practice.



## Tools Used



- Nmap 7.99.1

- Windows Command Prompt

- Windows 11



---



## 1. TCP Port Audit



A TCP SYN/connect-style audit was performed against the previously

identified local hosts.



### Command



nmap -sT -Pn -T3 192.168.1.1 192.168.1.2 192.168.1.3 192.168.1.7 192.168.1.10 192.168.1.15 -oN scans/week2-tcp-connect.txt



### Key Findings



#### 192.168.1.1



Open TCP ports:



- 53/tcp - DNS

- 80/tcp - HTTP

- 5555/tcp - Personal Agent / UPnP-related service



#### 192.168.1.2



- 7/tcp - filtered



#### 192.168.1.3



- Ports were largely filtered/no-response.



#### 192.168.1.7



- Ports were largely filtered/no-response.



#### 192.168.1.10



Open TCP ports:



- 135/tcp - MSRPC

- 139/tcp - NetBIOS

- 445/tcp - SMB

- 3306/tcp - MySQL



#### 192.168.1.15



- No open TCP ports were identified in the scanned default port set.



---



## 2. Service Fingerprinting



Service detection was performed on the hosts with identified

open TCP services.



### 192.168.1.1



The host exposed:



- DNS on port 53

- HTTP on port 80

- Personal Agent / UPnP-related service on port 5555



The host was associated with Boa HTTPd and dnsmasq-related services.



### 192.168.1.10



Service fingerprinting identified:



| Port | Service | Version / Information |

|------|---------|-----------------------|

| 135 | MSRPC | Microsoft Windows RPC |

| 139 | NetBIOS-SSN | Microsoft Windows netbios-ssn |

| 445 | Microsoft-DS | SMB-related service |

| 3306 | MySQL | MySQL (unauthorized) |



The host was identified as a Windows system.



No exploitation or credential attacks were performed.



---



## 3. UDP Audit



A targeted UDP scan of the top 20 ports was performed.



### Command



nmap -sU -Pn --top-ports 20 192.168.1.1 192.168.1.2 192.168.1.3 192.168.1.7 192.168.1.10 192.168.1.15 -oN scans/week2-udp.txt



### Observations



Several UDP ports were reported as closed or open|filtered.



Examples included:



- 137/udp - NetBIOS-NS

- 138/udp - NetBIOS-DGM

- 500/udp - ISAKMP

- 1900/udp - UPnP

- 4500/udp - NAT-T



Open|filtered means Nmap could not conclusively determine whether

the port was open because UDP services may not respond directly to

the probe.



---



## 4. Attack Surface Risk Matrix



A risk matrix was created based on the observed services.



| Host | Port | Protocol | Service | Risk | Recommendation |

|------|------|----------|---------|------|----------------|

| 192.168.1.1 | 53 | TCP | DNS | Medium | Restrict DNS access to trusted network |

| 192.168.1.1 | 80 | TCP | HTTP | Medium | Disable unused web service or harden HTTP |

| 192.168.1.1 | 5555 | TCP | UPnP/Personal-Agent | High | Restrict or disable unnecessary service |

| 192.168.1.10 | 135 | TCP | MSRPC | Medium | Restrict RPC access using firewall |

| 192.168.1.10 | 139 | TCP | NetBIOS | Medium | Disable NetBIOS if not required |

| 192.168.1.10 | 445 | TCP | SMB | High | Restrict SMB to trusted hosts |

| 192.168.1.10 | 3306 | TCP | MySQL | High | Restrict database access and require authentication |

| 192.168.1.10 | 137 | UDP | NetBIOS-NS | Medium | Restrict NetBIOS traffic |

| 192.168.1.10 | 138 | UDP | NetBIOS-DGM | Medium | Restrict NetBIOS traffic |

| 192.168.1.10 | 500 | UDP | ISAKMP | Medium | Verify whether IPsec is required |

| 192.168.1.10 | 1900 | UDP | UPnP | Medium | Disable UPnP if unnecessary |

| 192.168.1.10 | 4500 | UDP | NAT-T | Medium | Verify IPsec/NAT-T requirement |



The detailed risk matrix is stored in:



`risk-matrix/attack-surface-risk.csv`



---



## 5. Security Recommendations



1. Restrict management and infrastructure services to trusted hosts.

2. Disable unnecessary services such as unused UPnP or HTTP services.

3. Restrict SMB access using host-based or network firewalls.

4. Avoid exposing MySQL beyond the systems that require database access.

5. Disable legacy NetBIOS services when they are not required.

6. Review filtered ports and firewall rules regularly.

7. Keep exposed network services updated and securely configured.

8. Use strong authentication for administrative and database services.



---



## 6. Evidence Files



### TCP



`scans/week2-tcp-connect.txt`



### Service Fingerprinting



`scans/192.168.1.1-service-fingerprint.txt`



`scans/192.168.1.10-service-fingerprint.txt`



### UDP



`scans/week2-udp.txt`



### Risk Matrix



`risk-matrix/attack-surface-risk.csv`



### Screenshots



Evidence screenshots will be stored in:



`screenshots/`



---



## Ethical Scope



This assessment was performed only against the authorized local

network for educational and internship purposes.



No exploitation, password attacks, credential attacks, vulnerability

exploitation, or unauthorized access attempts were performed.



## Conclusion



The Week 2 assessment successfully performed TCP and UDP port auditing,

service fingerprinting, and attack-surface risk classification.



The exercise demonstrated practical network security assessment

techniques using Nmap and produced evidence files and security

recommendations based on the observed network exposure.

