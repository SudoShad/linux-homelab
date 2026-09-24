# Linux Homelab & Proxmox Lab Stack

Personal systems lab for practicing IT support and junior sysadmin work outside a classroom. Started as a self-hosted Ubuntu server (WireGuard VPN, firewall, Bash/Python automation) and expanded into a **Proxmox** virtualization lab that mirrors the stack on my résumé: Active Directory, Group Policy, Wazuh SIEM, osTicket, and remote access over WireGuard.

Configs and hostnames in this public repo are **documented practice / configs redacted** — secrets, domain credentials, and production-like keys are not published. The WireGuard and Ubuntu admin notes below reflect work done on the host; the Proxmox / AD / Wazuh sections describe lab architecture I run and maintain.

---

## Overview

| Component | Details |
|---|---|
| Hypervisor | Proxmox VE (lab VMs) |
| Host OS | Ubuntu Server (LTS) — WireGuard edge / automation host |
| Identity | Active Directory DC + Group Policy (lab) |
| SIEM | Wazuh (lab) |
| Help desk | osTicket (lab) |
| VPN | WireGuard |
| Firewall | iptables / ufw |
| Scripting | Bash, Python |
| Services | Self-hosted, managed via systemd |
| Remote Access | SSH + WireGuard tunnel |

---

## Lab architecture (Proxmox)

Documented practice on Proxmox — **configs redacted** from this public tree:

### Active Directory + Group Policy
- Windows Server domain controller VM joined to a lab domain
- Organizational units, security groups, and baseline Group Policy Objects for workstation lock screen, password policy, and software restriction practice
- Practice workflows: user create/disable, group membership, GPO link/unlink, and verifying policy application with `gpupdate` / Resultant Set of Policy

**Why:** Same identity and endpoint-policy patterns used in help desk / jr sysadmin roles (AD, GPO, account lifecycle).

### Wazuh SIEM
- Wazuh manager + agent practice for host log collection and alert triage
- Sample rules/alerts for authentication failures and service health (lab-only; no customer data)
- Used to build comfort reading SIEM alerts next to traditional sysadmin logs (`journalctl`, auth logs)

**Why:** Security is a longer-term direction; the SIEM sits alongside, not ahead of, core Windows/Linux admin practice.

### osTicket
- Self-hosted ticket queue for practicing intake, priority, assignment, and resolution notes
- Mirrors ServiceNow / Jira-style workflows used in IT support roles

### Networking & access
- Lab VMs reachable over the WireGuard tunnel and/or local LAN as configured
- Firewall rules (iptables/ufw) keep management paths intentional — default deny inbound on the Ubuntu edge host

---

## What's Running (Ubuntu host)

### WireGuard VPN
Configured a full WireGuard VPN server from scratch — no GUI, no wizards.

- Generated server and client keypairs (`wg genkey`, `wg pubkey`)
- Configured `/etc/wireguard/wg0.conf` with peers, allowed IPs, and DNS
- Brought interface up with `wg-quick up wg0` and enabled persistence via `systemd`
- Wrote `iptables` / `ufw` rules to allow VPN traffic on UDP 51820 and forward packets between the tunnel and LAN interface
- Verified handshakes and traffic routing with `wg show` and `ping` tests through the tunnel

**Why:** Secure encrypted remote access to the home network without exposing services to the public internet. Same fundamental workflow as enterprise VPN deployment.

---

### Firewall Configuration
- Default deny inbound policy via `ufw`
- Explicit allow rules for SSH (port 22), WireGuard (UDP 51820), and any running services
- Reviewed and tightened `iptables` FORWARD chain to control inter-interface routing
- Logging enabled for dropped packets to monitor unauthorized access attempts

---

### System Administration (Ongoing)
Day-to-day tasks that mirror a junior sysadmin workflow:

- **User management** — creating users, assigning groups, setting file permissions with `chmod` / `chown`
- **Service monitoring** — checking service health with `systemctl status`, restarting failed services, reviewing `journalctl` logs
- **Scheduled tasks** — writing and deploying `cron` jobs for automated maintenance
- **Package management** — `apt update`, `apt upgrade`, holding pinned packages, managing PPAs

---

### Automation Scripts
Bash and Python scripts written to reduce manual work:

- **Backup script (Bash)** — compresses and timestamps specified directories, logs success/failure, runs nightly via cron
- **Log parser (Python)** — scans auth logs for repeated failed SSH login attempts and outputs a summary report
- **System health check (Bash)** — checks disk usage, memory, and service status; sends output to a local log file

Related public tooling: [linux-ops-toolkit](https://github.com/SudoShad/linux-ops-toolkit) (POSIX `sh` backup / SSH guard / health check).

---

## Skills Demonstrated

- Proxmox virtualization and multi-VM lab design
- Active Directory domain admin basics and Group Policy (lab)
- Wazuh SIEM familiarity (lab triage)
- Help desk practice with osTicket
- Linux server administration (Ubuntu)
- VPN deployment and network tunneling (WireGuard)
- Firewall rule management (iptables, ufw)
- Bash and Python scripting for automation
- systemd service management
- SSH key-based authentication and hardening
- cron scheduling
- Log analysis and monitoring

---

## Why I Built This

Most IT certifications teach concepts. This lab is where I practice the actual commands, make mistakes, and fix them without a safety net. Host-level WireGuard and firewall work was done manually — so I understand what's happening at each layer — and the Proxmox stack adds AD/GPO, ticketing, and SIEM practice that map to IT Support → Jr Sysadmin roles.

Security (Wazuh, hardening) is intentional growth on top of that systems foundation, not the headline.

Next steps: tighter Intune / endpoint-management labs, more GPO baselines, and richer SIEM detection content — still with configs redacted in public docs.

---

## Related labs

- [helpdesk-graph-toolkit](https://github.com/SudoShad/helpdesk-graph-toolkit) — Entra / Graph helpdesk automation
- [endpoint-hardening-baseline](https://github.com/SudoShad/endpoint-hardening-baseline) — Intune Windows hardening policy-as-code
- [linux-ops-toolkit](https://github.com/SudoShad/linux-ops-toolkit) — POSIX `sh` backup / SSH guard / health check
- [ad-intune-mini-tenant](https://github.com/SudoShad/ad-intune-mini-tenant) — Entra + Intune enroll lab *(PARKED)*

---

## Connect

- Portfolio: [shadman.io](https://shadman.io)
- LinkedIn: [linkedin.com/in/shadman-bari](https://linkedin.com/in/shadman-bari)
- Email: shadman@shadman.io
- GitHub: [github.com/sudoshad](https://github.com/sudoshad)
