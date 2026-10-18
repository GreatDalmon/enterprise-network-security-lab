# Enterprise Network & Security Lab
![Network topology](Screenshots/DAY-3/topology-view-day 3.png)

A multi-phase enterprise networking and security project built in **Cisco Packet Tracer**. The project covers network design, VLAN segmentation, inter-VLAN routing, infrastructure services, network security, testing, troubleshooting, and documentation.

The project was developed as a practical environment for building networking and cybersecurity skills.

---

## 🎯 Project Aim

The aim of this project was to design and implement a functional enterprise network, secure the infrastructure, validate the security controls, and create a documented foundation for future cybersecurity testing.

The project follows:

**Network Design → Implementation → Services → Security → Validation → **

---

## 🏗️ Network Overview

The network uses a hierarchical design consisting of:

- Cisco 1941 Router
- Core switch
- HR access switch
- Sales access switch
- IT access switch
- Infrastructure server
- Multiple client PCs

### VLAN Structure

| VLAN | Department | Network | Gateway |
|---|---|---|---|
| 10 | HR | `192.168.10.0/24` | `192.168.10.1` |
| 20 | Sales | `192.168.20.0/24` | `192.168.20.1` |
| 30 | IT | `192.168.30.0/24` | `192.168.30.1` |
| 99 | Server / Management | `192.168.99.0/24` | `192.168.99.1` |

Inter-VLAN routing is provided using **Router-on-a-Stick**.

---

## 🌐 Network Services

The infrastructure server provides:

- DHCP
- DNS
- HTTP
- FTP
- SMTP
- POP3

DHCP relay was configured on the router so clients across the departmental VLANs could receive addresses from the central server.

---

## 🔐 Security Implementation

Security was introduced after the core network was operational.

### SSH

- SSH version 2 enabled
- Local user authentication
- VTY access restricted to SSH
- Telnet disabled
- Management access tested

### Port Security

Department access ports were configured with:

- Access mode
- Maximum of one MAC address
- Sticky MAC learning
- Violation mode set to `restrict`
- PortFast

Port-security violations were deliberately tested and verified.

### Access Control List

An extended ACL named `BLOCK-HR-SALES` was implemented to prevent direct communication between HR and Sales.

```text
deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip any any
