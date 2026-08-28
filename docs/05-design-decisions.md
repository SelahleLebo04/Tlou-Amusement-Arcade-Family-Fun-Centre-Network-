# 05 — Design Decisions

## Why VLAN segmentation instead of one flat network
Separating Admin, Arcade, Café, Guest, Print-zone, and Services into their own VLANs keeps POS/financial traffic away from patron devices, limits broadcast domains on a network with ~80 concurrent devices, and gives a clean point (the router) to apply access control between groups — which is exactly what CR8 needed.

## Why router-on-a-stick / single router with sub-interfaces
The client is a small single site with a modest budget and a bandwidth constraint. A single router handling inter-VLAN routing and NAT avoids the cost of a Layer 3 switch or multiple routers, while still giving each VLAN its own gateway and the ability to apply per-VLAN ACLs.

## Why VLSM instead of equal-sized subnets
Splitting the /24 evenly across 7 VLANs would waste addresses on small VLANs (Print-zone needs 4 hosts, Guest needs 40) and risk running out of room for Guest and Arcade growth. VLSM sizes each subnet to its actual need — largest (Guest, /26) to smallest (WAN, /30) — and leaves ~108 addresses unused for future growth.

## How CR8 (shared printer zone) is solved
Before this design, Arcade and Café had no printing at all. Rather than giving each department its own printer (extra hardware cost) or placing the printer on Admin's VLAN (unnecessary exposure of admin traffic), a dedicated PRINT-ZONE VLAN (50) was created. The router's inter-VLAN routing explicitly permits ARCADE (20) and CAFE (30) to reach PRINT-ZONE (50) on printing ports, while GUEST (40) and all other VLANs remain blocked from it. This satisfies "a shared printer zone must serve two departments that currently cannot print" without introducing scope beyond the wording of the change request.

## How the bandwidth constraint shaped the design
- A single WAN circuit — no redundant/expensive secondary link.
- Internal DHCP/DNS server so devices lease addresses and resolve local hostnames without relying on the WAN link.
- POS/arcade/café traffic stays entirely on the LAN (device ↔ local server), not routed to the internet.
- Guest Wi-Fi is isolated on its own VLAN, making it the natural (and only) place to apply rate-limiting/QoS, since it's the one user group expected to generate significant internet-bound traffic.
- No cloud-dependent services (e.g., cloud printing) — the printer zone added for CR8 is entirely local.

## How the IPv6 dual-stack challenge is addressed
Every VLAN gets a /64 IPv6 subnet alongside its IPv4 subnet, using a ULA prefix (`fd54:acde:2026::/48`) since no ISP-delegated global prefix is available in the lab environment. The router runs both stacks concurrently on each sub-interface — SLAAC + stateless DHCPv6 for user VLANs, static addressing for the printer zone, services VLAN, and WAN link — so IPv4 and IPv6 hosts are both natively routed rather than one being tunnelled inside the other.
