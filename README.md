# Proxmox-Homelab

A Proxmox-based homelab built for hands-on cybersecurity skill development — SOC tooling, IAM, and infrastructure hardening.

## Why I Built This

I'm transitioning into a cybersecurity career, and I wanted practical experience beyond coursework. This lab lets me deploy, break, and fix the same kinds of tools used in real security operations: SIEM and detection in a SOC context, and identity and access management with SSO. Documenting each build here turns that experience into a record of what I can actually do.

## Stack

- Proxmox VE
- pfSense
- AdGuard Home
- WireGuard
- Nginx Proxy Manager
- TrueNAS
- Authentik
- Uptime Kuma
- Vaultwarden
- BookStack
- n8n

## Related Projects

- [soc-lab](https://github.com/RakeemSec/soc-lab) — the dedicated, network-segmented SOC lab for detection engineering and IR practice

## Repository Structure

| Folder | Contents |
| --- | --- |
| `/authentik` | Authentik identity provider: SSO setup, providers, applications, and flows |
| `/networking` | Network design: pfSense, VLANs, firewall rules, DNS, and reverse proxy configs |
| `/docs` | General documentation, write-ups, and guides for the lab |
