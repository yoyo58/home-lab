# Lab 02 — Segmented network with a firewall

**Phase:** 0 — Foundation · **Date completed:** <YYYY-MM-DD> · **Time spent:** <hours>

## Objective

Understand routing, segmentation, and firewall rules — the backbone of defence. Put a firewall between two subnets and prove that rules do what I think they do, by testing and by reading the logs.

## Architecture & stack

- **Firewall/router:** pfSense CE _or_ OPNsense (both free), as a VM
- **Legs:** WAN (NAT, build-time), LAN-A `workstations` (10.10.10.0/24), LAN-B `servers` (10.10.20.0/24)
- **Test VMs:** one workstation on LAN-A, one Ubuntu server (web + SSH) on LAN-B
- **Diagram:** `../../diagrams/lab-02.png` _(add after build)_

## Build

1. **Deploy the firewall VM** with three adapters: WAN → NAT, LAN-A → internal `workstations`, LAN-B → internal `servers`.
2. **Assign interfaces and subnets** in the pfSense/OPNsense console, enable DHCP on each LAN.
3. **Place the test VMs** on their respective internal networks; confirm each gets an address and can reach the firewall.
4. **Write rules:** allow LAN-A → LAN-B on TCP/80 only; block LAN-A → LAN-B on TCP/22 (SSH); default-deny inter-LAN otherwise.

```bash
# From the workstation (LAN-A), against the server (LAN-B = 10.10.20.10)
curl -m3 http://10.10.20.10        # expect: succeeds (allowed)
nc -vz -w3 10.10.20.10 22          # expect: fails/timeout (blocked)
```

## Exercises & findings

1. **Rule verification** — run the curl/nc tests above; capture pass and fail. Evidence: `evidence/rule-tests.png`
2. **Read the logs** — find the blocked SSH attempt in the firewall's live log; note the rule that dropped it. Evidence: `evidence/firewall-log-block.png`
3. **Break & confirm** — temporarily disable the TCP/80 allow rule, show the web request now fails, re-enable it. Evidence: `evidence/break-confirm.png`
4. **Alias + NAT port-forward** — create an alias, add then remove a port-forward; note the log entries.

## Detection / defence angle

A firewall log is a detection source. Blocked inter-segment attempts (especially SSH/RDP from a workstation subnet) are exactly the kind of thing a SIEM should later alert on — this log feeds Phase 1.

## Lessons & next steps

- _Rule order / default-deny gotchas._
- Next: Lab 03 — stand up Active Directory behind this firewall.

## Artifacts in this folder

- `README.md` — this write-up
- `evidence/` — rule tests, firewall log showing a block, break/confirm
- `rules/` — exported rule set (optional)

---
_Part of the [Cybersecurity Home Lab](../../README.md) program._
