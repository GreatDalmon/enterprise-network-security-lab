# Validation & Troubleshooting

## Validation Checklist

| Test | Status |
|------|:------:|
| DHCP | ✅ |
| DNS Resolution | ✅ |
| HTTP Website | ✅ |
| FTP Login | ✅ |
| Email (SMTP/POP3) | ✅ |
| Inter-VLAN Routing | ✅ |
| SSH | ✅ |
| Port Security | ✅ |
| Sticky MAC Learning | ✅ |
| Port-Security Violation Test | ✅ |
| HR → Sales ACL Block | ✅ |
| Sales → HR ACL Block | ✅ |
| IT → HR Connectivity | ✅ |
| IT → Sales Connectivity | ✅ |
| IT → Server Connectivity | ✅ |

---

## Security Verification

### SSH

`show ip ssh` confirmed that SSH version 2 was enabled on the router. SSH was also verified on the core switch and access switches.

### ACL

`show access-lists` showed the `BLOCK-HR-SALES` ACL and recorded matches on both deny statements.

The ACL successfully blocked direct HR ↔ Sales traffic while allowing other required traffic.

### Port Security

`show port-security interface fa0/2` confirmed:

- Port Security: Enabled
- Port Status: Secure-up
- Violation Mode: Restrict
- Maximum MAC Addresses: 1
- Sticky MAC Addresses: 1

The security violation counter increased during the unauthorized/replacement device test, confirming that the violation policy was active.

---

## Connectivity Testing

### IT Testing

From an IT PC:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.99.2
```

All three tests succeeded.

### ACL Testing

From HR:

```text
ping 192.168.20.1
```

The test failed as expected because HR-to-Sales traffic is blocked.

Server access remained available so required infrastructure services were not unnecessarily disrupted.

---

## Issues Encountered

| Issue | Resolution |
|--------|------------|
| Router interfaces down | Enabled interfaces using `no shutdown`. |
| Native VLAN mismatch | Corrected trunk configuration between switches. |
| DHCP clients not receiving addresses | Corrected DHCP settings and verified DHCP relay using `ip helper-address`. |
| Some PCs had reverted to static addressing | Restored the affected PCs to DHCP. |
| DNS resolution failed | Added and corrected DNS records on the server. |
| Email authentication failed | Corrected usernames and passwords. |
| FTP authentication failed | Updated FTP user credentials. |
| HTTP webpage inaccessible | Verified HTTP service and DNS configuration. |
| Server ping initially failed | Checked VLAN/IP configuration and corrected affected endpoint settings. |
| Port-security test did not initially show the expected result | Verified the correct PC/port was being tested and checked sticky MAC and violation counters. |
| `show running-config interface fa0/2` was rejected in Packet Tracer | Used `show port-security interface fa0/2` and related verification commands instead. |

---

## Lessons Learned

- VLAN and IP addressing must be checked before troubleshooting higher-level services.
- DHCP configuration must remain consistent across all client PCs.
- Trunk ports and VLAN assignments must be verified carefully.
- Security controls should be tested after configuration rather than assumed to work.
- Sticky MAC addresses provide useful endpoint control on access ports.
- ACLs should be specific enough to enforce the security policy without breaking required services.
- Troubleshooting incorrect test endpoints can prevent unnecessary configuration changes.

---

## Evidence

Day 3 security evidence is stored in the project screenshots folder, including:

- SSH and ACL verification
- Port-security verification
- Sticky MAC addresses
- Security connectivity tests
- Day 3 network topology
