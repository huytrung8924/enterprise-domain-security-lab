# Evidence index

[← Project overview](../README.md)

Source: the supplied **84-page system-build report**. Page numbers are 1-based PDF pages and match the printed page numbers. The image ordinal is its order among embedded image blocks on that page. Images are preserved directly from PDF bytes without resampling or content edits.

| Image | PDF page | Image order | Description |
|---|---|---|---|
| [topology](../screenshots/architecture/topology.png) | 1 | 1 | Original system topology |
| [opnsense-console](../screenshots/architecture/opnsense-console.png) | 5 | 1 | OPNsense interface console |
| [domain-join](../screenshots/identity/domain-join.png) | 13 | 1 | Windows client domain join |
| [security-groups](../screenshots/identity/security-groups.png) | 16 | 1 | IT security group |
| [it-access-allowed](../screenshots/identity/it-access-allowed.png) | 20 | 1 | IT user accesses IT share |
| [it-access-denied](../screenshots/identity/it-access-denied.png) | 20 | 2 | IT user denied access to Sales share |
| [sales-access-allowed](../screenshots/identity/sales-access-allowed.png) | 21 | 1 | Sales user accesses Sales share |
| [sales-access-denied](../screenshots/identity/sales-access-denied.png) | 21 | 2 | Sales user denied access to IT share |
| [ldap-query](../screenshots/identity/ldap-query.png) | 25 | 2 | LDAP user query result |
| [dns-validation](../screenshots/services/dns-validation.png) | 29 | 1 | DNS lookup validation |
| [mail-received](../screenshots/services/mail-received.png) | 36 | 2 | Message received by Sales mailbox |
| [iis-lan-access](../screenshots/services/iis-lan-access.png) | 37 | 2 | IIS site access from LAN |
| [nat-forward](../screenshots/services/nat-forward.png) | 38 | 1 | HTTP port forwarding rule |
| [aspgoat-access](../screenshots/services/aspgoat-access.png) | 41 | 1 | ASPGoat access from Kali |
| [internal-hostname](../screenshots/services/internal-hostname.png) | 42 | 1 | Website access by internal hostname |
| [password-policy](../screenshots/gpo/password-policy.png) | 44 | 1 | Configured password policy |
| [password-rejected](../screenshots/gpo/password-rejected.png) | 44 | 2 | Password rejected by policy |
| [lockout-policy](../screenshots/gpo/lockout-policy.png) | 45 | 1 | Configured account lockout policy |
| [account-locked](../screenshots/gpo/account-locked.png) | 45 | 2 | Account lockout result |
| [audit-validation](../screenshots/gpo/audit-validation.png) | 47 | 1 | Client audit policy validation |
| [drive-mapping](../screenshots/gpo/drive-mapping.png) | 48 | 1 | Drive-mapping policy |
| [software-installed](../screenshots/gpo/software-installed.png) | 50 | 2 | 7-Zip installation verification |
| [unikey-running](../screenshots/gpo/unikey-running.png) | 51 | 3 | UniKey running after logon |
| [client-connected](../screenshots/vpn/client-connected.png) | 64 | 1 | OpenVPN client connection completed |
| [lan-reachability](../screenshots/vpn/lan-reachability.png) | 64 | 2 | VPN interface routes and LAN ping results |
| [wazuh-overview](../screenshots/monitoring/wazuh-overview.png) | 67 | 1 | Wazuh dashboard overview |
| [server-agent](../screenshots/monitoring/server-agent.png) | 68 | 3 | Windows Server agent active |
| [client-agent](../screenshots/monitoring/client-agent.png) | 69 | 1 | Windows client agent active |
| [opnsense-agent-config](../screenshots/monitoring/opnsense-agent-config.png) | 70 | 2 | OPNsense Wazuh Agent settings |
| [sysmon-events](../screenshots/monitoring/sysmon-events.png) | 73 | 2 | Sysmon events from Windows endpoints |
| [suricata-rule](../screenshots/syn-flood/suricata-rule.png) | 74 | 1 | SYN-flood drop rule |
| [opnsense-blocked](../screenshots/syn-flood/opnsense-blocked.png) | 74 | 3 | Suricata blocked events on OPNsense |
| [wazuh-events](../screenshots/syn-flood/wazuh-events.png) | 74 | 4 | SYN-flood events in Wazuh |
| [sensitive-uri-rule](../screenshots/directory-enumeration/sensitive-uri-rule.png) | 75 | 1 | Sensitive URI detection |
| [get-rate-rule](../screenshots/directory-enumeration/get-rate-rule.png) | 76 | 1 | HTTP GET rate detection |
| [http-404-rule](../screenshots/directory-enumeration/http-404-rule.png) | 76 | 2 | HTTP 404 rate detection |
| [correlation-rule](../screenshots/directory-enumeration/correlation-rule.png) | 76 | 3 | Combined xbits detection rule |
| [wazuh-rule](../screenshots/directory-enumeration/wazuh-rule.png) | 77 | 1 | Wazuh level-12 detection rule |
| [active-response](../screenshots/directory-enumeration/active-response.png) | 77 | 2 | OPNsense firewall Active Response |
| [gobuster-alert](../screenshots/directory-enumeration/gobuster-alert.png) | 78 | 2 | Gobuster-related alert in Wazuh |
| [firewall-alias](../screenshots/directory-enumeration/firewall-alias.png) | 79 | 1 | Attacker IP in firewall alias |
| [dirsearch-alert](../screenshots/directory-enumeration/dirsearch-alert.png) | 80 | 1 | Dirsearch-related alert in Wazuh |
| [dirsearch-firewall-alias](../screenshots/directory-enumeration/dirsearch-firewall-alias.png) | 80 | 2 | Firewall alias after the Dirsearch exercise |
| [fim-configuration](../screenshots/web-shell/fim-configuration.png) | 81 | 1 | IIS webroot FIM configuration |
| [fim-rules](../screenshots/web-shell/fim-rules.png) | 81 | 3 | Web-shell file detection rules |
| [sysmon-rules](../screenshots/web-shell/sysmon-rules.png) | 82 | 1 | Sysmon process and network detection rules |
| [command-result](../screenshots/web-shell/command-result.png) | 83 | 3 | Command execution result in the lab web shell |
| [wazuh-alerts](../screenshots/web-shell/wazuh-alerts.png) | 84 | 1 | FIM command and network alerts in Wazuh |

Images document separate configuration and validation moments. They should not be interpreted as a continuous synchronized timing trace. The full PDF and secret-bearing installation/profile artifacts are not included in the repository.
