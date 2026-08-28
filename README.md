# CMPG 325 — Individual Network Project
### Tlou Amusement Arcade & Family Fun Centre (Vryburg)

**Student:** Selahle, Lebo
**Student number:** 47925019
**Project ID:** CMPG325-2026-124 | **Client ID:** CLI-124
**Industry:** Entertainment
**Assigned addressing block:** 192.168.54.0/24
**Assigned technical challenge:** IPv6 (dual-stack addressing & routing)
**Design constraint:** Internet bandwidth is limited and expensive — design minimises WAN-dependent traffic
**Change request (CR8):** A shared printer zone must serve two departments (Arcade + Cafe) that currently cannot print

---

## Project summary

Tlou Amusement Arcade & Family Fun Centre is a small entertainment venue combining a coin-operated arcade floor, a café/snack bar, and a back-office administration function. This repository documents the end-to-end network design and simulation delivered for the client: requirements analysis, physical and logical topology, IPv4/IPv6 addressing, Packet Tracer implementation, the assigned IPv6 dual-stack challenge, the CR8 shared-printer fix, and testing evidence.

## Repository structure

```
.
├── README.md                          # you are here
├── docs/
│   ├── 01-client-requirements.md      # requirements analysis
│   ├── 02-physical-topology.svg       # physical device/cabling diagram
│   ├── 03-logical-topology.svg        # VLAN / IP logical diagram
│   ├── 04-ip-addressing-plan.md       # IPv4 VLSM plan + IPv6 dual-stack plan
│   └── 05-design-decisions.md         # justifications for key design choices
├── packet-tracer/
│   └── tlou-network.pkt               # working Packet Tracer file
├── testing/
│   ├── connectivity-tests.md          # test plan + results
│   └── screenshots/                   # evidence captures
└── reflection.md                      # short reflection on the completed project
```

## Milestones

| Milestone | Date | Status |
|---|---|---|
| Project commencement | 14 Aug 2026 | ✅ |
| Milestone 1 — Client Design Review | 28 Aug 2026 | ✅ this submission |
| Milestone 2 — Client Implementation Review | 02 Oct 2026 | ⬜ pending |
| Final submission | 16 Oct 2026 | ⬜ pending |

## Quick links

- [Client requirements](docs/01-client-requirements.md)
- [Physical topology](docs/02-physical-topology.svg)
- [Logical topology](docs/03-logical-topology.svg)
- [IP addressing plan](docs/04-ip-addressing-plan.md)
- [Design decisions](docs/05-design-decisions.md)


