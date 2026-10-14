# Lab 03 — Active Directory domain

**Phase:** 0 — Foundation · **Date completed:** <YYYY-MM-DD> · **Time spent:** <hours>

## Objective

Build the enterprise environment that the vast majority of real attacks and defences revolve around: a working Active Directory domain with users, groups, OUs and Group Policy. This same domain becomes the victim in Phase 3, so I keep a clean snapshot.

## Architecture & stack

- **Domain Controller:** Windows Server 2022 Evaluation (180-day, free)
- **Clients:** 1–2× Windows 11 Enterprise Evaluation
- **Domain:** `lab.local` (example) · all on the isolated `internal` network from Lab 1
- **Telemetry:** Sysmon installed on every host (config comes in Lab 5, but install it now)
- **Diagram:** `../../diagrams/lab-03.png` _(add after build)_

## Build

1. **Install Windows Server**, set a static IP on the `internal` network, rename the host (e.g. `DC01`).
2. **Promote to a Domain Controller** — add the *Active Directory Domain Services* role, create a new forest `lab.local`. Reboot.
3. **Design the directory:** OUs (e.g. `Workstations`, `Servers`, `Staff`, `Admins`), 15–20 users, a few groups.
4. **Group Policy:** a password policy, a logon banner, and a mapped drive GPO — enough to see GPO processing in action.
5. **Join the Windows 11 clients** to `lab.local`; log in as domain users.
6. **Create a service account** and a file share with NTFS permissions.
7. **Install Sysmon** on DC and clients now (default config for now; curated config in Lab 5).
8. **Snapshot everything** as `clean-domain` — this is the Phase 3 starting point.

```powershell
# On the server, after prerequisites — promote to a new forest
Install-ADDSForest -DomainName "lab.local" -InstallDns
# Verify afterwards
Get-ADDomain ; Get-ADUser -Filter * | Measure-Object
```

> **Shortcut:** **GOAD / GOAD-Light** (Orange Cyberdefense) automates a deliberately-vulnerable AD forest and is ideal for Phase 3 (Lab 18). Build at least one domain by hand first, though — it teaches far more about how AD actually works. GOAD-Light needs ~20 GB RAM and ~115 GB disk.

## Exercises & findings

1. **GPO verification** — run `gpupdate /force` on a client, confirm the banner and mapped drive apply. Evidence: `evidence/gpo-applied.png`
2. **Permissions test** — confirm a normal user is denied, a group member allowed, on the file share. Evidence: `evidence/ntfs-perms.png`
3. **Directory overview** — screenshot the OU/user structure. Evidence: `evidence/ad-structure.png`

## Detection / defence angle

Every logon, GPO change, and account creation here is an event the SIEM will consume in Phase 1. Note which Windows Security Event IDs correspond to logons (4624/4625), account creation (4720), and group changes (4728/4732).

## Lessons & next steps

- _DNS pointing, static IP, eval-licence expiry reminders._
- Next: Phase 1, Lab 04 — ship these logs into a SIEM.

## Artifacts in this folder

- `README.md` — this write-up
- `evidence/` — GPO applied, NTFS perms, AD structure
- OU/GPO design notes (optional)

---
_Part of the [Cybersecurity Home Lab](../../README.md) program._
