# 01 — Client Requirements Analysis

**Client:** Tlou Amusement Arcade & Family Fun Centre (Vryburg) | **Industry:** Entertainment
**Assigned block:** 192.168.54.0/24 | **Challenge:** IPv6 dual-stack addressing & routing
**Constraint:** Internet bandwidth is limited and expensive — design must minimise waste
**CR8:** A shared printer zone must serve two departments that currently cannot print

## Business overview
Tlou Amusement Arcade & Family Fun Centre is a small, single-site entertainment venue. The site combines a coin-operated arcade gaming floor, a café/snack bar for patrons, and a small back-office administration function. As a small business, the network needs to be simple to manage, cheap to run, and resilient enough that a single fault doesn't take down ticketing, POS, or the arcade floor during trading hours.

## Departments / user groups
| Department | Function | Approx. devices |
|---|---|---|
| Administration | Manager PC, bookkeeping, staff rostering, back-office printing | 5 |
| Arcade gaming floor | Redemption/POS kiosks, arcade machine network controllers | 20 |
| Café & snack bar | POS tills, kitchen order display | 8 |
| Guest Wi-Fi | Patron personal devices (phones/tablets) | 40 (concurrent) |
| Shared services | Internal DHCP/DNS server, switch management | 5 |
| Shared printer zone (CR8) | Printer(s) shared by Arcade + Café, who currently have none | 4 |

## Applications & services
- POS/till processing (Arcade + Café) — reliable LAN connectivity, low internet dependency
- Back-office productivity (Admin) — file sharing, printing, email
- Guest internet access (Wi-Fi) — general browsing, isolated from internal systems
- Internal DHCP and DNS — reduces reliance on external services for local resolution
- Shared network printing (new, per CR8)

## Security considerations
- Guest Wi-Fi is logically isolated (separate VLAN/subnet) from POS, admin, and printer traffic — no client-to-client visibility, no route to internal servers.
- The printer zone created for CR8 is reachable only from the Arcade and Café VLANs — the two departments it serves — not from Guest Wi-Fi.
- The Admin subnet holds the most sensitive data (financials, staff records) and has the least exposure to guest/public traffic.

## Budget & bandwidth constraint
- Single, appropriately-sized WAN link — no redundant/expensive secondary circuit.
- Internal DHCP/DNS server so devices resolve local names and lease addresses without depending on the WAN link.
- POS/arcade traffic stays on the LAN rather than routing to the internet.
- Guest Wi-Fi is the only group expected to consume significant internet bandwidth, so it's the candidate for rate-limiting/QoS on the router.
- No unnecessary internet-dependent services (e.g., cloud printing) — printing stays entirely local.

## Growth
The IPv4 addressing plan reserves roughly 40% of the assigned /24 as unallocated space, so additional arcade machines, a second café till lane, or a future kiosk area can be added without renumbering the network.
