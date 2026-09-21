# Homelab Deployment Gap List
_Compiled 2026-09-21 — current Proxmox inventory (`lab-pve-01`) vs. what's documented in `Proxmox-Homelab` / `Soc-Lab` and prior planning notes. Intended as a build/fix backlog for Claude Code._

## Current confirmed inventory (lab-pve-01)
LXCs: AdGuard (100), Uptime Kuma (101), Nginx Proxy Manager (102), Jellyfin (104), Ubuntu (105), BookStack (108), WordPress (110, stopped), WireGuard (111)
VMs: TrueNAS-01 (103), Ubuntu25.10 (106), Kali (107), HAOS (109), vm-Docker (113)
Storage: localnetwork, LOGS-BACKUPS, local, local-lvm

---

## 1. Soc-Lab stack — documented, not yet deployed
| Item | Repo location | Notes |
|---|---|---|
| Wazuh (Manager/Indexer/Dashboard) | `/wazuh` | Core of the whole SOC lab pipeline — highest priority |
| TheHive | `/thehive` | Depends on Wazuh alerts feeding in |
| Shuffle (SOAR) | `/shuffle` | Webhooks Wazuh alerts → TheHive |
| Cortex | `/cortex` | Enrichment, cross-refs MISP |
| MISP | `/misp` | Threat intel source for Cortex |
| Windows Server 2022 DC | `/active-directory` | For Kerberoasting/DCSync detection (MITRE T1558/T1003) |
| Windows 10/11 client (Sysmon+FIM, Wazuh agent) | `/active-directory` or new folder | In progress — fresh build today on the newly created dedicated SOC-lab VLAN, not a repurpose of 105/106 |
| Linux client (auditd+FIM, Wazuh agent) | — | In progress — fresh build today on the newly created dedicated SOC-lab VLAN, not a repurpose of 105/106 |

Kali (107) is already in place and satisfies the "attack simulation" role — no action needed there. The dedicated SOC-lab VLAN (separate from the general homelab's Lab VLAN, per the earlier segmentation decision) has now been created — endpoint builds are underway on it as of today.

## 2. Documented as deployed, but not visible on lab-pve-01 — confirm status/location
| Item | Where it's claimed | Discrepancy |
|---|---|---|
| n8n | Proxmox-Homelab repo Stack; earlier notes say "self-hosted on Proxmox LXC" for the AI News Autopilot | Confirmed gone — needs a fresh LXC reinstall; the AI News Autopilot workflow will need rebuilding once it's back up |
| Vaultwarden | Proxmox-Homelab repo Stack | Not its own container — likely a third docker-compose service inside vm-Docker (113) alongside Portainer/Authentik; confirm and document |
| Portainer | Confirmed running inside vm-Docker via `docker run` | Not mentioned in Proxmox-Homelab README's Stack section at all — add once other placements are confirmed |

**Confirmed:** vm-Docker's docker-compose stack (Portainer, Authentik, and Vaultwarden going forward) is the intended long-term pattern for these lighter-weight services — no need to carve out dedicated LXCs for them.

## 3. In progress
- **Grafana** — actively being deployed today. Once live, update Soc-Lab's "Planned Additions" section to move it into the actual Stack.

## 4. Planned, not started
- Prometheus, Loki, Graylog (observability layer)
- Lynis + CIS Debian Benchmark hardening pass on the PVE host (with before/after Nessus validation) — no repo/folder decided yet
- Public-facing writeup for the **Nessus vulnerability management program** (NIST 800-53 / ISO 27001 / CIS v8 aligned, six-phase methodology) — currently has zero public documentation; needs a new sanitized repo or section (no raw scan data, per the established rule)
- Proxmox Backup Server (PBS) — confirmed still queued, not yet installed (LOGS-BACKUPS/local/local-lvm are unrelated storage, not PBS)
- Second Proxmox node (spare Beelink S12 Pro, N100/16GB) + Raspberry Pi QDevice for a two-node cluster — only one node (`lab-pve-01`) currently exists in Datacenter

## 5. Repo hygiene fixes (documentation, not deployment)
- **Soc-Lab `/snort` folder** contradicts its own README, which states Snort/Suricata "lives in the general homelab's networking layer, not in this repo." Either move that folder's contents into Proxmox-Homelab's `/networking` or delete it, or update the README note if it's staying.
- **Proxmox-Homelab Stack list** doesn't mention WireGuard at all — it's the actual deployed VPN (111) and should replace any lingering OpenVPN reference from earlier planning.