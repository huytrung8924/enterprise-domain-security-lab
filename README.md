# 🛡️ Enterprise Domain Infrastructure & Security Lab

![Platform](https://img.shields.io/badge/Platform-VMware_Workstation-607078)
![Windows](https://img.shields.io/badge/Windows_Server-2022-0078D4)
![Firewall](https://img.shields.io/badge/Firewall-OPNsense-EF7B00)
![Network Security](https://img.shields.io/badge/Network_Security-Suricata-C74228)
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-0078D4)

**A domain-based enterprise lab combining centralized Windows administration, business services, perimeter security, remote access, and security monitoring.**

This project builds a Windows domain on **`trungnh.local`**, separates WAN and LAN through **OPNsense**, inspects traffic with **Suricata**, and collects endpoint and firewall telemetry in **Wazuh**. **Sysmon** adds Windows process and network visibility.

The implementation covers more than attack detection: it includes AD DS, DNS, LDAP queries, departmental SMB permissions, internal email, IIS and ASPGoat, Group Policy, OpenVPN, and three documented security demonstrations.

## 📊 What was achieved

| Area | Implemented capability | Recorded validation |
|---|---|---|
| Domain infrastructure | AD DS, DNS, Windows client domain membership | Domain-join confirmation and directory queries |
| Access control | IT/Sales groups, SMB share and NTFS permissions | Allowed own-department access and denied cross-department access |
| Business services | hMailServer, IIS, ASPGoat, internal DNS names | Received email and successful web access |
| Central administration | Password, lockout, audit, drive mapping, software, and logon-script GPOs | Rejected weak password, locked account, audit settings, 7-Zip and UniKey results |
| Perimeter and remote access | OPNsense routing/NAT, Suricata, OpenVPN | Blocked network alerts and VPN connectivity to LAN |
| Monitoring | Wazuh all-in-one, Windows/OPNsense agents, Sysmon collection | Active Windows agents, configured firewall agent, Sysmon and Suricata events |
| Security demos | SYN flood, web directory enumeration, IIS web shell | Network blocking, firewall IP containment, and endpoint alerts |

## 🏗️ Architecture

![Original WAN firewall and domain LAN topology](screenshots/architecture/topology.png)

| Machine | Platform | IP address | Role |
|---|---|---|---|
| Kali Linux | Kali Linux | `192.168.249.130` | WAN-side test host and VPN client |
| OPNsense | OPNsense | WAN `192.168.249.134`; LAN `192.168.10.1` | Gateway, NAT, firewall, Suricata, OpenVPN |
| Windows Server | Windows Server 2022 | `192.168.10.10` | AD DS, DNS, LDAP, SMB, IIS, hMailServer |
| Windows client | Windows 10 | `192.168.10.20` | Domain workstation and policy validation |
| Wazuh server | Ubuntu Server 22.04 LTS | `192.168.10.30` | Wazuh Manager, Indexer, and Dashboard |

**WAN:** VMware VMnet8 NAT, `192.168.249.0/24`. **LAN:** VMware VMnet2, `192.168.10.0/24`.

**Monitoring:** Windows logs and Sysmon → Wazuh Agents → Wazuh Manager → Wazuh Indexer → Wazuh Dashboard. Suricata events on OPNsense reach the same Wazuh platform through the firewall's Wazuh Agent integration.

[Architecture and network inventory →](docs/architecture.md)

## 🏢 Infrastructure results

### Domain identity and departmental file access

AD DS provides centralized identities and the client joins `trungnh.local`. IT and Sales users are grouped separately, with access enforced through share and NTFS permissions.

![IT user successfully accesses the IT shared folder](screenshots/identity/it-access-allowed.png)

![IT user is denied access to the Sales folder](screenshots/identity/it-access-denied.png)

**Recorded result:** `it01` accesses IT data and is denied Sales access. The report also includes the reciprocal `sale01` test. LDAP queries return directory attributes for `it01`.

[Domain, LDAP, DNS, and SMB evidence →](docs/domain-and-services.md)

### Centralized workstation management

Group Policy configures password and lockout controls, auditing, drive mapping, MSI deployment, and a UniKey logon script.

![Domain account lockout after failed authentication attempts](screenshots/gpo/account-locked.png)

**Recorded result:** a password that fails the configured policy is rejected; the report records account lockout after five failed attempts. Client audit settings, automatic 7-Zip installation, and UniKey at logon are also documented.

[GPO configuration and validation →](docs/group-policy.md)

### Internal services and remote access

The lab validates internal email delivery, IIS/ASPGoat access, web hostname resolution, and HTTP publishing through OPNsense. OpenVPN provides a remote route into the LAN.

![VPN tunnel interface routing and successful LAN connectivity tests](screenshots/vpn/lan-reachability.png)

**Recorded result:** the VPN client receives **`10.10.0.2/24`**, has a route to the LAN, and receives ping replies from **`192.168.10.1`** and **`192.168.10.10`**.

[Service results →](docs/domain-and-services.md) · [Firewall and VPN →](docs/firewall-and-vpn.md)

## 🔎 Centralized monitoring

Wazuh runs as an **all-in-one deployment**. Windows Server and Windows client agents collect Windows telemetry; Sysmon events from both hosts are visible in Discover. The OPNsense Wazuh Agent integration supplies firewall-side IDS events.

![Sysmon events from both Windows hosts in Wazuh Discover](screenshots/monitoring/sysmon-events.png)

[Monitoring architecture and evidence →](docs/monitoring.md)

## 🌐 Demo 01 — SYN-flood detection and blocking

**Objective:** detect a TCP SYN burst directed at the published web service and verify Suricata's blocking action on OPNsense.

### Dashboard and SIEM logs

![SYN-flood alerts in Wazuh Threat Hunting](screenshots/syn-flood/wazuh-events.png)

### Network enforcement evidence

![OPNsense Suricata events showing blocked action](screenshots/syn-flood/opnsense-blocked.png)

### Key results

| Evidence | Observed result |
|---|---|
| Detection rule | Suricata SID **9000020**, `drop` action, TCP SYN to `$HTTP_SERVERS:80` |
| Configured threshold | Source-based `detection_filter`, count **300**, **5-second** window |
| SIEM event | `Suricata: Alert - DOS Possible SYN Flood`, Wazuh rule **86601** |
| Enforcement | OPNsense alert rows show **blocked**, source `192.168.249.130`, destination `192.168.10.10:80` |

[Full walkthrough and supporting evidence →](demos/01-syn-flood/README.md)

## 🗂️ Demo 02 — Web directory enumeration and IP containment

**Objective:** combine sensitive-path requests, high HTTP GET activity, and repeated 404 responses, then block the scanning source through Wazuh Active Response on OPNsense.

### Dashboard and SIEM logs

![Web directory enumeration alert at level 12 in Wazuh](screenshots/directory-enumeration/gobuster-alert.png)

### Firewall enforcement evidence

![Scanning source added to the OPNsense firewall alias](screenshots/directory-enumeration/firewall-alias.png)

### Key results

| Evidence | Observed result |
|---|---|
| Detection design | Suricata `xbits` aggregate three indicators before a combined alert |
| Wazuh rule | **100600**, level **12**, Web Directory Enumeration Attack |
| Response | `opnsense-fw` on the OPNsense agent, configured timeout **300 seconds** |
| Enforcement | Source **`192.168.249.130`** appears in firewall alias **`__wazuh_agent_drop`**; a separate alias capture accompanies the Dirsearch test |
| Tools documented | Gobuster and Dirsearch have separate alert evidence |

[Full walkthrough and supporting evidence →](demos/02-directory-enumeration/README.md)

## 🐚 Demo 03 — IIS web-shell detection

**Objective:** detect webroot file changes and suspicious command/network activity during a controlled IIS web-shell exercise.

### Dashboard and SIEM logs

![Wazuh alerts for IIS file changes command execution and network connection](screenshots/web-shell/wazuh-alerts.png)

### Execution evidence

![Recorded ipconfig output from the lab web-shell exercise](screenshots/web-shell/command-result.png)

### Key results

| Evidence | Observed result |
|---|---|
| File monitoring | FIM alerts for webroot file creation and modification: **100500**, **100501** |
| Behavior monitoring | Command execution **100530** and network connection **100531** alerts |
| Visible severity | The displayed event rows have level **12** |
| Outcome | File changes and suspicious execution/network activity are visible together in Wazuh; automatic removal or blocking is not demonstrated for this scenario |

[Full walkthrough and supporting evidence →](demos/03-web-shell/README.md)

## 📂 Repository guide

| Path | Contents |
|---|---|
| [`docs/architecture.md`](docs/architecture.md) | Networks, machines, and traffic/log paths |
| [`docs/domain-and-services.md`](docs/domain-and-services.md) | AD DS, LDAP, DNS, SMB, mail, IIS, ASPGoat |
| [`docs/group-policy.md`](docs/group-policy.md) | GPO configuration and client verification |
| [`docs/firewall-and-vpn.md`](docs/firewall-and-vpn.md) | OPNsense, NAT, Suricata, OpenVPN |
| [`docs/monitoring.md`](docs/monitoring.md) | Wazuh Agent and Sysmon integration |
| [`docs/results.md`](docs/results.md) | Outcome matrix and validation scope |
| [`docs/evidence-index.md`](docs/evidence-index.md) | Screenshot-to-PDF mapping |
| [`demos/`](demos/) | Three security walkthroughs |
| [`screenshots/`](screenshots/) | Original embedded report images |
| [`docs/github-publishing.md`](docs/github-publishing.md) | Upload instructions and repository metadata |

## 🧠 Technical takeaways

- Directory administration and security monitoring complement each other: domain identities, policy, services, and telemetry share the same lab environment.
- Share and NTFS permissions need validation with both allowed and denied access tests.
- Network alerts, endpoint alerts, and firewall enforcement are different evidence sources.
- The two blocking demos use different mechanisms: Suricata packet dropping and Wazuh-triggered OPNsense IP containment.
- Sysmon and FIM reveal different parts of the web-shell exercise: process/network activity and webroot file changes.

## 🔭 Next steps

Version the original configuration exports and GPO backups, record full component versions, export raw events, test response timeout expiry, and measure false positives, service availability, and end-to-end response latency.

## Evidence basis

This repository documents the supplied **84-page system-build report**. Images retain their original embedded content. Detailed source pages are listed in the [evidence index](docs/evidence-index.md). It is a documentation and evidence portfolio; the report does not supply a complete source-code or deployment export. Displayed event totals are time-window snapshots rather than accuracy or performance measurements.
