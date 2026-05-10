# Linux Homelab & VPN Infrastructure

Personal homelab running on a self-hosted Ubuntu Linux server. Built to practice real-world system administration, network security, and automation outside of a controlled lab environment. Everything here mirrors workflows found in production IT environments.

---

## Overview

| Component | Details |
|---|---|
| OS | Ubuntu Server (LTS) |
| VPN | WireGuard |
| Firewall | iptables / ufw |
| Scripting | Bash, Python |
| Services | Self-hosted, managed via systemd |
| Remote Access | SSH + WireGuard tunnel |

---

## What's Running

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

---

## Skills Demonstrated

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

Most IT certifications teach concepts. This lab is where I practice the actual commands, make mistakes, and fix them without a safety net. Every configuration here was done manually — no Ansible, no GUI — so I understand what's happening at each layer.

The goal is to keep expanding it: next steps include setting up a local DNS resolver, experimenting with containerized services via Docker, and adding centralized log monitoring.

---

## Connect

- Portfolio: [shadman.io](https://shadman.io)
- LinkedIn: [linkedin.com/in/shadman-bari](https://linkedin.com/in/shadman-bari)
- Email: shadman@shadman.io
