# 04 — IP Addressing Plan

## VLAN design
| VLAN | Name | Purpose |
|---|---|---|
| 10 | ADMIN | Back-office PCs, printing |
| 20 | ARCADE | Kiosk/POS terminals on the gaming floor |
| 30 | CAFE | Café POS terminals |
| 40 | GUEST | Patron Wi-Fi, isolated from all internal VLANs |
| 50 | PRINT-ZONE | Shared printers (CR8) — reachable only from VLAN 20 and VLAN 30 |
| 60 | SERVICES | Internal DHCP/DNS server, switch management |
| 99 | WAN | Point-to-point link to the ISP |

## IPv4 addressing (VLSM over 192.168.54.0/24)
| VLAN | Subnet | Mask | Usable range | Gateway | Hosts needed | Hosts available |
|---|---|---|---|---|---|---|
| 40 GUEST | 192.168.54.0/26 | 255.255.255.192 | .1 – .62 | 192.168.54.1 | 40 | 62 |
| 20 ARCADE | 192.168.54.64/27 | 255.255.255.224 | .65 – .94 | 192.168.54.65 | 20 | 30 |
| 30 CAFE | 192.168.54.96/28 | 255.255.255.240 | .97 – .110 | 192.168.54.97 | 8 | 14 |
| 10 ADMIN | 192.168.54.112/28 | 255.255.255.240 | .113 – .126 | 192.168.54.113 | 5 | 14 |
| 60 SERVICES | 192.168.54.128/29 | 255.255.255.248 | .129 – .134 | 192.168.54.129 | 5 | 6 |
| 50 PRINT-ZONE | 192.168.54.136/29 | 255.255.255.248 | .137 – .142 | 192.168.54.137 | 4 | 6 |
| 99 WAN (to ISP) | 192.168.54.144/30 | 255.255.255.252 | .145 – .146 | — | 2 | 2 |
| Reserved for growth | 192.168.54.148 – .255 | — | — | — | — | 108 addresses |

VLANs were ordered largest-to-smallest before assigning blocks (standard VLSM practice) so the /24 packs efficiently with no wasted overlap.

## IPv6 addressing (dual-stack — assigned challenge)
Using a locally-assigned ULA prefix `fd54:acde:2026::/48` (swap for an ISP-delegated global prefix if one becomes available), subnetted into /64s per VLAN, matching the IPv4 VLAN structure:

| VLAN | IPv6 subnet | Assignment method |
|---|---|---|
| 10 ADMIN | fd54:acde:2026:10::/64 | SLAAC + stateless DHCPv6 (DNS) |
| 20 ARCADE | fd54:acde:2026:20::/64 | SLAAC + stateless DHCPv6 |
| 30 CAFE | fd54:acde:2026:30::/64 | SLAAC + stateless DHCPv6 |
| 40 GUEST | fd54:acde:2026:40::/64 | SLAAC + stateless DHCPv6 |
| 50 PRINT-ZONE | fd54:acde:2026:50::/64 | Static (printers) |
| 60 SERVICES | fd54:acde:2026:60::/64 | Static (server, router interfaces) |
| 99 WAN | fd54:acde:2026:ffff::/64 | Static (router-to-ISP, /127 also acceptable) |

Router interfaces take `::1` in each subnet as the IPv6 gateway (e.g., `fd54:acde:2026:20::1` for VLAN 20). Both stacks run concurrently on every router sub-interface, so IPv4 and IPv6 traffic are both routed natively — not tunnelled — satisfying the dual-stack requirement.
