# Results and validation scope

[← Project overview](../README.md)

## Infrastructure

| Capability | Evidence in report | Result supported |
|---|---|---|
| AD DS and domain membership | Pages 9–16 | Domain configured and client joined |
| Departmental SMB access | Pages 17–21 | IT/Sales own-folder access and cross-folder denial |
| LDAP search | Pages 22–26 | Bind/search workflow and returned AD attributes |
| DNS | Pages 26–30 | Service records and lookup validation |
| Internal mail | Pages 29–36 | Message received in recipient mailbox |
| IIS and ASPGoat | Pages 36–41 | Web deployment and internal/WAN-side access tests |
| GPO | Pages 42–51 | Password/lockout/audit validation and desktop-management outcomes |
| OpenVPN | Pages 55–64 | Tunnel established, routes present, LAN gateway/server reachable |
| Wazuh and Sysmon | Pages 65–73 | Agents deployed and Windows telemetry searchable |

## Security demonstrations

| Scenario | Detection | Enforcement shown | Evidence |
|---|---|---|---|
| SYN flood | Suricata SID 9000020; Wazuh 86601 | Suricata action marked blocked | Page 74 |
| Directory enumeration | Suricata xbits; Wazuh 100600, level 12 | Firewall alias entries | Pages 75–80 |
| IIS web shell | Wazuh FIM and Sysmon rules | Detection only | Pages 80–84 |

## Values interpreted accurately

- SYN rule count **300** over **5 seconds** is configuration, not a measured capacity limit.
- Directory response timeout **300 seconds** is configured; expiry was not timed in the supplied evidence.
- Web-shell event rows show **100500**, **100501**, **100530**, and **100531**; the configured content rule 100502 is not claimed as observed in the final table.
- VPN results include address **10.10.0.2/24** and successful replies from the LAN gateway and DC.
- Dashboard hit totals depend on the filter and selected time window. They do not establish accuracy, unique attack count, or latency.

## Evidence boundaries

The report supplies deployment descriptions and screenshots, not complete configuration exports, raw-event datasets, or benchmark scripts. No new lab execution was performed to create this portfolio. Exact build versions, detection latency, false-positive rate, production availability, formal CIS/NIST compliance, and timeout rollback remain unverified.
