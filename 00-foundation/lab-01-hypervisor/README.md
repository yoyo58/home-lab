# Lab 01 — Hypervisor & base images

**Phase:** 0 — Foundation · **Date completed:** <YYYY-MM-DD> · **Time spent:** <hours>

## Objective

Stand up a repeatable VM workflow with clean, isolated networking and working snapshots. This is the ground every other lab runs on: if networking and snapshots are solid here, nothing downstream fights me.

## Architecture & stack

- **Hypervisor:** VMware Workstation Pro 17 (free for personal use) _or_ VirtualBox 7.x
- **Base VMs:** Kali Linux (attacker), Ubuntu Server 24.04 (generic Linux target)
- **Virtual networks:**
  - `NAT` — build-time internet only, used deliberately then disconnected
  - `host-only` — isolated lab, no internet, no route to home LAN
  - `internal` — lab-to-lab only (used from Lab 3 onward)
- **Diagram:** `../../diagrams/lab-01.png` _(add after build)_

## Build

1. **Install the hypervisor.**
   - VMware: download *Workstation Pro 17 for Personal Use* from the Broadcom portal (free Broadcom account required), install, and at first run choose **"Use VMware Workstation 17 for Personal Use."**
   - VirtualBox: install the base package + the matching Extension Pack.
2. **Define the three virtual networks** in the hypervisor's Virtual Network Editor (VMware) / Host Network Manager (VirtualBox): one NAT, one host-only (DHCP on, no uplink), one internal.
3. **Build the Kali VM** — 2 vCPU / 4 GB RAM / 60 GB disk. Install, update once over NAT, then move its adapter to host-only.
4. **Build the Ubuntu Server VM** — 2 vCPU / 2 GB RAM / 25 GB disk. Minimal install, OpenSSH enabled.
5. **Snapshot both** as `clean-install` once each boots and updates cleanly.

```bash
# Prove the host-only network has no internet (run inside a host-only VM)
ping -c2 8.8.8.8        # expect: 100% packet loss
ip route                # expect: no default route via the host-only adapter
```

## Exercises & findings

1. **Clone a VM** from the clean snapshot and confirm the clone is independent. Evidence: `evidence/clone.png`
2. **Revert a snapshot** — break something (delete a package), then revert and confirm it's back. Evidence: `evidence/snapshot-revert.png`
3. **Network isolation test** — move the Kali VM between NAT and host-only; confirm internet works on NAT and fails on host-only. Evidence: `evidence/isolation-test.png`

## Detection / defence angle

Isolation *is* the control here. The whole program's safety depends on host-only/internal networks genuinely having no path out — verify it rather than assume it.

## Lessons & next steps

- _What tripped me up (bridged-vs-host-only confusion, DHCP, etc.)._
- Next: Lab 02 — put a firewall between segments.

## Artifacts in this folder

- `README.md` — this write-up
- `evidence/` — screenshots of clone, snapshot revert, isolation test

---
_Part of the [Cybersecurity Home Lab](../../README.md) program._
