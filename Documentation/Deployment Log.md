# Deployment Log

## Day 1 — Office Network

### Tasks Completed

- Built the initial network topology.
- Connected router, switch, server, and client PCs.
- Configured router interfaces.
- Assigned a static IP to the server.
- Configured DHCP.
- Configured DNS records.
- Enabled HTTP service.
- Configured FTP.
- Configured SMTP and POP3 email services.
- Tested network connectivity.

---

## Day 2 — Enterprise Expansion

### Tasks Completed

- Redesigned the network into a hierarchical topology.
- Added a core switch and departmental access switches.
- Created VLANs for HR, Sales, IT, and Servers.
- Configured trunk links.
- Implemented Router-on-a-Stick.
- Configured DHCP relay.
- Updated DNS records.
- Verified HTTP, FTP, and Email services.
- Validated inter-VLAN routing.

---

## Day 3 — Network Security

### SSH

- Enabled SSH version 2.
- Configured the local `admin` user.
- Configured the domain name required for SSH.
- Restricted VTY access to SSH.
- Verified SSH using `show ip ssh`.

### Port Security

Port security was implemented on HR, Sales, and IT access switches.

Configuration included:

```text
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation restrict
spanning-tree portfast
```

Sticky MAC addresses were learned and verified.

A replacement/unauthorized endpoint test was performed on an HR access port. The violation counter increased while the port remained `Secure-up`, confirming that `restrict` mode was working.

### ACL

An extended ACL named `BLOCK-HR-SALES` was created:

```text
deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip any any
```

The ACL was applied inbound to the HR and Sales router subinterfaces.

Testing confirmed:

- HR → Sales: blocked
- Sales → HR: blocked
- IT → HR: allowed
- IT → Sales: allowed
- IT → Server: allowed
- Required server connectivity remained available

### Key Verification Commands

```text
show ip ssh
show access-lists
show ip interface brief
show vlan brief
show interfaces trunk
show port-security address
show port-security interface fa0/2
ping
ftp
```

---

## Current Version

**Version:** 3.0

**Status:** Stable and security-tested
