# Network Infrastructure

## Overview

| Layer | Hardware | Role |
|---|---|---|
| Gateway | Eero Pro mesh | 1 Gbps fiber — WiFi throughout |
| Core switch | TP-Link TL-SG116 (16-port unmanaged) | LAN backbone in SMC |
| Patch panel | VCELINK 24-port Cat6A keystone | Structured wiring termination in SMC |
| Inter-floor backbone | ScreenBeam MoCA adapters + quad-shield RG6 coax | Ethernet over coax between floors |

**SMC contents:** Mini S13 PC (HAOS) · MoCA adapter · TP-Link switch.

---

## Cabling

Mix of Cat6 (most runs) and MoCA (floors without direct Cat6). Quad-shield RG6 coax on MoCA runs between floors provides reliable gigabit-class throughput without new wiring.

---

## Zigbee Coordinator Placement

**ZBT-2 USB dongle → Sabrent USB 2.0 hub → extension cable → away from Eero**

Zigbee 2.4 GHz signal competes with WiFi channels 1, 6, and 11. Channel 25 (chosen) maps to ~2.475 GHz — clear of all standard WiFi channels. Physical separation from the Eero gateway reduces interference further.

!!! warning "Coordinator placement matters"
    Keep the ZBT-2 dongle at least 1 meter from the Eero and any other 2.4 GHz radio. A USB extension cable solves this cheaply.

---

## Zigbee Mesh Best Practices

1. **Pair mains-powered devices first** (switches, plugs) — they form the mesh backbone that battery sensors route through
2. **Wait 24 hours** after adding powered devices before testing signal — neighbor tables rebuild gradually
3. **Battery-powered devices** (sensors, remotes) rejoin automatically after coordinator restarts via channel scan
4. **ZHA coordinator migration:** Use the ZHA radio migration tool — no re-pairing required
5. **Channel 25** deliberately chosen to avoid 2.4 GHz WiFi overlap

---

## IP Addressing

HA instance local IP is stored in `secrets.yaml` (not in repo). Reference: `192.168.x.x`.

All SSH/SCP commands in CLAUDE.md and this documentation use `192.168.x.x` as a placeholder — substitute the real IP from `secrets.yaml`.
