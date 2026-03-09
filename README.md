# Active Directory Detection Lab

A homelab environment for practicing Active Directory administration, attack simulation, and blue team detection using Wazuh SIEM. Built as a portfolio project covering real-world MITRE ATT&CK techniques and detection engineering.

> **WARNING:** This is an isolated lab environment. All credentials are placeholders. Never reuse lab passwords in production or internet-facing systems.

---

## Architecture Overview

![Lab Architecture](architecture/lab-architecture-diagram.png)

| VM | Hostname | IP Address | OS | RAM | Role |
|---|---|---|---|---|---|
| dc01 | DC01 | 192.168.56.10 | Windows Server 2016 | 4 GB | Primary Domain Controller, DNS |
| dc02 | DC02 | 192.168.56.102 | Windows Server 2016 | 2 GB | Replica Domain Controller |
| wkstn01 | WKSTN01 | 192.168.56.20 | Windows 10 Pro | 4 GB | Domain Workstation |
| siem01 | siem01 | 192.168.56.103 | Ubuntu 24.04 LTS | 4 GB | Wazuh SIEM Server |

**Network:** VirtualBox Host-Only Adapter (`192.168.56.0/24`)  
**Domain:** `corp.techcorp.internal` | NetBIOS: `TECHCORP` | Functional level: Windows Server 2016

See [architecture/network-topology.md](architecture/network-topology.md) for the full network and port matrix.

---

## Tech Stack

| Component | Version | Purpose |
|---|---|---|
| VirtualBox | 7.x | Hypervisor |
| Windows Server 2016 | -- | Domain Controllers |
| Windows 10 Pro | 22H2 | Workstation |
| Ubuntu Server | 24.04 LTS | SIEM host |
| Wazuh | 4.7.5 | SIEM / detection |
| Ansible | 2.15+ | Windows automation |
| PowerShell | 5.1 | AD provisioning and attack simulation |

---

## MITRE ATT&CK Coverage

| # | Technique | ID | Tactic | Wazuh Rule |
|---|---|---|---|---|
| 1 | Kerberoasting | T1558.003 | Credential Access | 100001 |
| 2 | Password Spray | T1110.003 | Credential Access | 100002 |
| 3 | Privilege Escalation | T1078 | Privilege Escalation | 100003 |
| 4 | Lateral Movement (SMB) | T1021.002 | Lateral Movement | 100004 |
| 5 | Rogue Account Creation | T1136.001 | Persistence | 100005 |

---

## Quick Start

1. [Prerequisites and software requirements](docs/02-prerequisites.md)
2. [Network configuration (VirtualBox)](docs/03-network-configuration.md)
3. [WinRM and Ansible setup](docs/04-winrm-ansible-setup.md)
4. [Active Directory setup](docs/05-active-directory-setup.md)
5. [Wazuh SIEM deployment](docs/06-wazuh-siem-setup.md)
6. [Wazuh agent and log configuration](docs/07-wazuh-agent-configuration.md)
7. [Detection rules](docs/08-detection-rules.md)
8. [Attack simulation guide](detections/attack-simulation-guide.md)

Reference:
- [Useful commands](docs/useful-commands.md)
- [Common issues and fixes](docs/common-issues.md)

---

## Reproducibility Notes

This repo keeps credentials out of tracked files and supports reproducible automation:

- Use `ansible/inventory.ini.template` and local overrides for environment-specific values.
- Keep real passwords in environment variables or untracked local files.
- Put real ISO paths and local network values in untracked `terraform.tfvars`.
- Keep Terraform state and secret artifacts ignored via `.gitignore`.

---

## Repository Structure

```text
ad-detection-lab/
├── README.md
├── LICENSE
├── .gitignore
├── architecture/           # Diagrams and topology docs
├── docs/                   # Step-by-step setup guides
├── scripts/
│   ├── powershell/         # AD setup and attack simulation scripts
│   ├── bash/               # Wazuh install and config scripts
│   └── config/             # ossec.conf snippet and detection rules XML
├── detections/             # Attack simulation guide and MITRE mapping
├── screenshots/            # Build and validation evidence
└── ansible/                # Inventory templates and playbooks
```

---

## Screenshots

86 sequential screenshots document the full build from prerequisites through Wazuh detections. See [screenshots/README.md](screenshots/README.md).

---

## License

MIT - see [LICENSE](LICENSE)
