# Energy & Power

## Time-of-Use Pricing

Dominion Energy charges different rates depending on the time of day. During "peak" hours, electricity costs roughly twice as much per kilowatt-hour as off-peak times.

| Period | Summer rate | Winter rate | When |
|---|---|---|---|
| **Peak** | $0.30/kWh | $0.26/kWh | Weekday afternoons/mornings (varies by season) |
| Off-peak | $0.14/kWh | $0.15/kWh | Most of the day |
| Super off-peak | $0.12/kWh | $0.14/kWh | Midnight – 5 AM daily |

**Peak hours:**

| Season | Peak window | Days |
|---|---|---|
| Summer (May–Sep) | 3:00 PM – 6:00 PM | Weekdays only |
| Winter (Oct–Apr) | 6:00 AM – 9:00 AM and 5:00 PM – 8:00 PM | Weekdays only |

No peak periods on weekends — the cheaper off-peak rate applies all day.

---

## How the House Saves Money Automatically

### Thermostat Pre-Conditioning (Eco+ TOU)

The Ecobee thermostats know the peak schedule. Before peak hours start, the system pre-cools (summer) or pre-heats (winter) the house so the HVAC runs less during the expensive window.

See [Climate & Comfort](climate.md#energy-saving-during-peak-hours) for more detail on how this works.

### Dashboard Peak Indicator

The Climate tab shows whether you're currently in a peak, off-peak, or super off-peak window and the live rate in $/kWh. Useful for deciding when to run appliances (dishwasher, laundry, etc.) — off-peak and super off-peak windows are much cheaper.

---

## What's Being Monitored Now

Four smart plugs throughout the house track live power draw and cumulative energy use:

| Location | What's plugged in |
|---|---|
| Front room | — |
| Upstairs hallway | — |
| Upstairs hallway (2nd plug) | — |
| Office | — |

These report live wattage and total kilowatt-hours to the dashboard and are logged to Grafana for historical trending.

---

## Whole-Home Energy Monitoring — Coming Soon

Two **Leviton EM16P** dual-panel energy monitors are in hand, waiting to be installed in the electrical panels. Once installed:

- Every circuit in the house gets its own energy sensor
- Total house consumption is visible in real time
- Historical usage is logged and viewable in Grafana
- This data feeds into more advanced TOU optimization

This is the single highest-ROI pending install — it unlocks detailed understanding of where the house's energy is going.

---

## Utility Meter Monitoring

A **NooElec NESDR SMArt v5** RTL-SDR dongle is connected to the HAOS machine and actively scanning 915 MHz for Itron ERT/AMR meter broadcasts.

### Electric Meter — Not Available

The Dominion Energy electric meter uses **AMI** (Advanced Metering Infrastructure) — a two-way cellular/mesh network that does not broadcast continuously on 915 MHz. The RTL-SDR cannot read it. Electric sub-metering will come from the EM16P clamp monitors once those are installed on the electrical panels.

### Gas Meter — Live

Columbia Gas of Virginia uses the older one-way **AMR** system, which broadcasts on 915 MHz every 30–60 seconds. The rtlamr2mqtt add-on is running, filtering for meter ID **12868632** (Itron ERT module on a Sensus R-275 meter body).

| Entity | Description |
|---|---|
| `sensor.gas_meter_reading` | Live cumulative ft³ reading, published to MQTT via rtlamr2mqtt auto-discovery |
| `sensor.gas_consumption_ccf` | Derived template sensor: ft³ ÷ 100 = CCF (same cumulative total, different unit) |
| `sensor.gas_daily` | Daily delta — resets to 0 at midnight; shows today's ft³ usage |
| `sensor.gas_monthly` | Monthly delta — resets on the 1st; shows this month's ft³ usage |

- All four sensors are live in HA
- `sensor.gas_meter_reading` and `sensor.gas_consumption_ccf` are wired into the HA Energy dashboard under Gas Consumption
- Meter ID confidence: high — consumption (~860,000 ft³) matches the physical register reading and it's the most frequently detected ID in neighborhood scans
- An automation (`gas_meter_restart_rtlamr_on_start`) restarts rtlamr2mqtt 30 s after every HA start to force a fresh MQTT publish (workaround for MQTT retain not being available in the add-on schema)

**Pending:** Gas anomaly alert and monthly cost summary automations (Phase 4 — blocked on first bill to confirm meter ID and get rate).
