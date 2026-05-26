# Energy Monitoring

## Strategy

Six workstreams, executed in order. Each builds on the previous.

| Workstream | Name | Status |
|---|---|---|
| WS0 | Entity setup + baseline discovery | **Blocked — EM16P not yet installed** |
| WS1 | 30-day passive baseline monitoring | Not started |
| WS2 | Financial reporting (cost tracking) | Deferred |
| WS3 | Advanced TOU optimization | Deferred |
| WS4 | Demand response | Deferred |
| WS5 | Future loads (EV, solar, battery) | Deferred — no EV or solar yet |

---

## EM16P Whole-Home Energy Monitors

### Status: Hardware in hand — **OVERDUE since May 22**

2× Leviton EM16P dual-panel energy monitors. Cover both electrical panels:

- Panel 1: Main house
- Panel 2: TBD

After install:
- HA discovers energy entities for each circuit
- WS0: identify entities, confirm statistics sensors, build whole-home template sensors
- WS1 begins automatically — 30 days of passive monitoring

---

## TOU Rate Sensors (Live)

Dominion Energy Schedule 1G — Virginia. Re-evaluated every minute.

| Entity | Current value | Description |
|---|---|---|
| `sensor.tou_period` | `peak` / `off_peak` / `super_off_peak` | Current billing period |
| `sensor.tou_rate` | $/kWh | Current rate |
| `input_boolean.peak_mode` | `on` / `off` | True during peak hours — used by dashboard |

### Rate Schedule

| Period | Summer rate | Winter rate |
|---|---|---|
| Super off-peak (midnight–5AM) | $0.1247/kWh | $0.1408/kWh |
| Off-peak | $0.1426/kWh | $0.1470/kWh |
| Peak | $0.3016/kWh | $0.2622/kWh |

Peak hours: Summer 3–6 PM weekdays · Winter 6–9 AM + 5–8 PM weekdays · No peak on weekends.

---

## Plug-Level Monitoring (Live)

4 smart plugs with power metering active:

| Location | Power (live) | Energy (cumulative) |
|---|---|---|
| Front room | `sensor.front_room_plug_1_power` | `sensor.front_room_plug_1_summation_delivered` |
| Upstairs hallway | `sensor.upstairs_hallway_1_power` | `sensor.upstairs_hallway_1_summation_delivered` |
| Upstairs hallway #2 | `sensor.upstairs_hallway_plug_2_power` | `sensor.upstairs_hallway_plug_2_summation_delivered` |
| Office | `sensor.office_plug_1_power` | `sensor.office_plug_1_summation_delivered` |

!!! note
    Inovelli switches do **not** report power consumption — smart plugs remain the primary sub-circuit monitoring layer until EM16P whole-panel monitoring is live.

---

## Grafana Analytics

InfluxDB 1.x receives all HA state changes. Grafana (running as HA add-on) provides time-series dashboards using **InfluxQL** queries.

Active panels:
- Temperature by zone (line chart)
- CO₂, VOC, AQI air quality (line chart)
- Humidity (line chart)
- Live power draw per plug (line chart)
- kWh totals per plug (stat panels)
- Garage door activity timeline
- Presence — who's home timeline
- Leak sensor history timeline
- Phone battery levels

See `grafana_queries.md` / `GRAFANA_SETUP.md` in the repo for query reference.
