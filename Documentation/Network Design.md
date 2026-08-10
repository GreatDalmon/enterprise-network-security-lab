# Network Design

## Architecture

The network follows a hierarchical design consisting of:

- 1 Cisco 1941 Router
- 1 Core Switch
- 3 Department Access Switches
- 1 Infrastructure Server
- Multiple Client PCs

The departments are segmented using VLANs while infrastructure services are hosted on a dedicated Server VLAN.

---

## VLAN Plan

| VLAN | Department | Network | Gateway |
|------|------------|----------------|----------------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 99 | Servers / Management | 192.168.99.0/24 | 192.168.99.1 |

---

## Infrastructure Services

| Service | Server |
|----------|--------|
| DHCP | SRV1-INFRA |
| DNS | SRV1-INFRA |
| HTTP | SRV1-INFRA |
| FTP | SRV1-INFRA |
| SMTP | SRV1-INFRA |
| POP3 | SRV1-INFRA |

The infrastructure server uses the Server VLAN and provides centralized services to the departmental VLANs.

---

## Device Inventory

| Device | Purpose |
|---------|---------|
| R1-CORE | Inter-VLAN Routing and Security |
| CORE-SW1 | Core / Distribution |
| HR-SW1 | HR Access Switch |
| SALES-SW1 | Sales Access Switch |
| IT-SW1 | IT Access Switch |
| SRV1-INFRA | Network Services |

---

## Security Design

### SSH

SSH version 2 is enabled for secure remote management of the router and switches. Local user authentication is used instead of unsecured remote management.

### Port Security

Department access ports are configured with:

- Access mode
- Port security
- Maximum of one secure MAC address
- Sticky MAC learning
- Violation mode set to `restrict`
- PortFast

This prevents an unauthorized device from freely replacing the expected endpoint.

### ACL

An extended ACL named `BLOCK-HR-SALES` is applied to the HR and Sales router subinterfaces.

Policy:

| Traffic | Result |
|---------|--------|
| HR → Sales | ❌ Blocked |
| Sales → HR | ❌ Blocked |
| HR → Server | ✅ Allowed |
| Sales → Server | ✅ Allowed |
| IT → HR | ✅ Allowed |
| IT → Sales | ✅ Allowed |
| IT → Server | ✅ Allowed |

The ACL includes `permit ip any any` after the specific HR/Sales deny rules so required services and other legitimate traffic continue to work.

---

## Design Decisions

- VLANs separate departments and reduce unnecessary broadcast traffic.
- Router-on-a-Stick provides inter-VLAN communication through a single router interface.
- Infrastructure services are centralized on a dedicated Server VLAN.
- DHCP relay allows the central DHCP service to provide addresses to departmental VLANs.
- SSH provides secure device management.
- Port security reduces the risk of unauthorized endpoint connections.
- ACLs provide controlled communication between departments.
- A dedicated lab connection can be reserved for the later cybersecurity testing phase.

## Management Access

- Username: "admin"
- Authentication: Local user database
- Remote access: SSH v2
- Telnet: Disabled

Credentials are not included in this repository.
