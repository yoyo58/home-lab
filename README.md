# Cybersecurity Home Lab

A six-month, hands-on home-lab program to build a strong, T-shaped cybersecurity profile — blue team, DFIR, offensive/purple, hardening, and cloud/DevSecOps. Each lab produces one documented artifact. This repo is the evidence.

> **Why this exists:** most entry-level candidates have a certificate and nothing to show behind it. This repo is 26 documented projects showing I can actually do the work.

## How it's organised

Six phases, roughly one lab per week. Each phase has its own folder; each lab has its own sub-folder with a `README.md` write-up plus an `evidence/` directory and any rules/scripts/configs it produced.

```
home-lab/
├── 00-foundation/        # hypervisor, segmented network, Active Directory
├── 01-blue-team/         # SIEM, Sysmon, NSM, detections, hunting, TI/SOAR
├── 02-dfir-malware/      # disk & memory forensics, static & dynamic malware
├── 03-offensive-purple/  # web, network, AD attacks, vuln mgmt, purple team
├── 04-hardening/         # benchmarks, IDS/IPS, VPN & PKI
├── 05-cloud-devsecops/   # cloud security, containers/K8s, CI/CD pipeline
├── diagrams/             # architecture & attack-path diagrams (draw.io)
└── _templates/           # the write-up template reused for every lab
```

The three strongest pieces — the Sigma rules (Lab 07), the purple-team report (Lab 20), and the DevSecOps pipeline (Lab 26) — also live in their own standalone repos for visibility, linked from the checklist below.

## Lab architecture

_Diagram to be added to `diagrams/` after Lab 1. In short: an isolated internal lab network behind a pfSense/OPNsense firewall, running a small Active Directory domain, a SIEM, and network monitoring, with a Kali attacker box and purpose-built analysis VMs attached per phase._

**Host:** a single Windows laptop — 16 GB RAM, 500+ GB SSD, VMware Workstation Pro. Every lab is sized to fit about 11 GB of VMs running at once, which is why some labs use lighter variants (Security Onion in Import mode, GOAD MINILAB). Doing it all on modest hardware is part of the exercise.

## Progress tracker

Tick a lab only when its write-up is published. 26 labs total.

### Phase 0 — Foundation
- [ ] Lab 01 — Hypervisor & base images
- [ ] Lab 02 — Segmented network with a firewall
- [ ] Lab 03 — Active Directory domain

### Phase 1 — Blue team / SOC
- [ ] Lab 04 — SIEM & log pipeline
- [ ] Lab 05 — Endpoint telemetry with Sysmon
- [ ] Lab 06 — Network security monitoring
- [ ] Lab 07 — Detection engineering with Sigma → _standalone repo_
- [ ] Lab 08 — Threat hunting
- [ ] Lab 09 — Honeypot
- [ ] Lab 10 — Phishing & malicious-document analysis
- [ ] Lab 11 — Threat intelligence & SOAR

### Phase 2 — DFIR & malware
- [ ] Lab 12 — Disk & triage forensics
- [ ] Lab 13 — Memory forensics
- [ ] Lab 14 — Static malware analysis
- [ ] Lab 15 — Dynamic malware analysis

### Phase 3 — Offensive & purple
- [ ] Lab 16 — Web application exploitation
- [ ] Lab 17 — Network exploitation fundamentals
- [ ] Lab 18 — Active Directory attacks
- [ ] Lab 19 — Vulnerability management
- [ ] Lab 20 — Purple team: attack meets detection → _standalone repo_

### Phase 4 — Hardening & infrastructure
- [ ] Lab 21 — System hardening to a benchmark
- [ ] Lab 22 — IDS/IPS & host firewalls
- [ ] Lab 23 — VPN, PKI & secure access

### Phase 5 — Cloud, container & DevSecOps
- [ ] Lab 24 — Cloud security fundamentals
- [ ] Lab 25 — Container & Kubernetes security
- [ ] Lab 26 — DevSecOps pipeline → _standalone repo_

## Safety

Every lab runs on an isolated network. Vulnerable VMs and malware never touch the internet or the host. Only systems I own are ever attacked. See each lab's write-up for its specific isolation setup.

## About

Built by [@yoyo58](https://github.com/yoyo58) as a structured path into a cybersecurity career. Write-ups follow the template in [`_templates/LAB_TEMPLATE.md`](_templates/LAB_TEMPLATE.md).
