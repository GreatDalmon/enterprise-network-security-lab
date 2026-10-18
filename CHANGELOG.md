# Changelog
## ## Changelog

## v3.0 — Network Security

* Enabled and verified SSH on the router and switches.
* Added port security to HR, Sales, and IT access ports.
* Configured sticky MAC addresses with a maximum of one MAC address per port.
* Configured port-security violation mode to "restrict".
* Tested port-security violations successfully.
* Added the "BLOCK-HR-SALES" extended ACL.
* Blocked direct communication between HR and Sales.
* Verified IT could still communicate with HR, Sales, and the server.
* Verified required server services remained accessible.
* Fixed PCs that had reverted from DHCP to static IP configuration.
* Completed security verification and testing.
* Added Day 3 screenshots and configuration files to the project documentation.

## v2.0 - Enterprise Network

### Added
- Core switch
- Three access switches
- VLAN 10, 20, 30, 99
- Router-on-a-Stick
- Centralized DHCP
- DNS
- HTTP
- FTP
- Email

### Fixed
- Native VLAN mismatch
- DHCP relay configuration
- DNS resolution
- FTP authentication
- Email authentication

---

## v1.0 - Office Network

### Added
- Single LAN
- DHCP
- DNS
- HTTP
- FTP
- Email
