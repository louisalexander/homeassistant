# RTL-SDR Utility Monitoring

## Goal

Add real-time gas and electric meter readings to Home Assistant using a single NooElec NESDR SMArt v5 RTL-SDR dongle. Both meters broadcast on 915 MHz using the Itron ERT/AMR protocol. Sensor data will feed the HA Energy dashboard and enable gas cost tracking and consumption anomaly alerts.

---

## Hardware

| Item | Details | Status |
|---|---|---|
| NooElec NESDR SMArt v5 | RTL-SDR dongle, 100 kHz–1.75 GHz | In hand |
| Host | Mini S13 PC running HAOS | Installed |

The dongle plugs directly into a USB port on the HAOS machine. No additional hardware required.

---

## Utility & Meter Details

| Utility | Provider | Protocol | Meter Type |
|---|---|---|---|
| Electric | Dominion Energy Virginia | Itron ERT/AMR — SCM or SCM+ | TBD (confirm via scan) |
| Gas | Columbia Gas of Virginia | Itron ERT/AMR — SCM or SCM+ | TBD (confirm via scan) |

**Zip code:** 23059

**Rate schedules:**
- Electric: Dominion Energy Schedule 1G (TOU) — rates already in HA via `sensor.tou_rate`
- Gas: Columbia Gas of Virginia — volumetric rate, no TOU; rate in $/therm (enter manually from bill)

**Meter IDs:** Must be discovered via Phase 1 passive scan. Cross-reference against account numbers on utility bills.

> **Note:** Dominion Virginia has been rolling out AMI (Advanced Metering Infrastructure) meters in some areas. AMI meters do not broadcast continuously — they respond only to utility queries over a proprietary mesh. If the electric meter is AMI it will not appear in an rtlamr scan. Confirm by running Phase 1 — if the meter ID appears, it is AMR-capable.

---

## Software Architecture

```
NooElec dongle (USB)
  └─► rtl_tcp (HAOS add-on: "RTL-SDR")
        └─► rtlamr (embedded in rtlamr2mqtt)
              └─► MQTT broker (Mosquitto, already in HA)
                    └─► Home Assistant (MQTT auto-discovery)
                          └─► Energy dashboard + automations
```

### Add-ons Required

| Add-on | Source | Purpose |
|---|---|---|
| Mosquitto broker | HA official add-on store | MQTT message bus (may already be installed) |
| rtlamr2mqtt | Custom repository (see setup) | Decodes AMR broadcasts → MQTT |

**rtlamr2mqtt repository URL:** `https://github.com/allangood/rtlamr2mqtt`

Add via: Settings → Add-ons → Add-on Store → ⋮ → Repositories → paste URL.

---

## Protocol Reference

| Term | Meaning |
|---|---|
| ERT | Encoder Receiver Transmitter — the physical layer on Itron meters |
| AMR | Automatic Meter Reading — one-way broadcast, no authentication |
| SCM | Standard Consumption Message — meter reading broadcast every ~30–60 sec |
| SCM+ | Enhanced SCM — same data, slightly different frame format; more common on newer meters |
| IDM | Interval Data Message — 15-min interval usage data; requires different decoder |

rtlamr decodes SCM and SCM+ automatically. Meter ID and consumption (in register units) are embedded in each broadcast.

**Gas units:** cubic feet (ft³). Convert to therms: 1 therm ≈ 100 ft³ (use actual BTU content from bill for precision).

---

## Phase 1 — Passive Scan (Discover Meter IDs)

**Goal:** Confirm both meters broadcast on 915 MHz and record their exact Meter IDs.

### Steps

1. Plug the NooElec dongle into a USB port on the HAOS machine.

2. Install the `RTL-SDR` driver add-on from the official HA add-on store (provides USB passthrough and `rtl_tcp`). Start it.

3. SSH into HAOS and run a 2-minute passive scan to capture all nearby Itron broadcasts:
   ```bash
   # From HAOS SSH terminal — requires rtlamr installed
   # If not available natively, use the rtlamr2mqtt add-on in "test" mode (see its docs)
   rtlamr -msgtype=all -duration=2m 2>/dev/null
   ```

4. Save the output. Look for records with `Type: SCM` or `Type: SCM+`. Each line includes:
   - `ID` — the meter's unique identifier
   - `Consumption` — current register reading
   - `CommodityType` — 4=gas (typically), 7=electric (check against Itron docs)

5. Cross-reference IDs against your utility bills:
   - Electric: Meter number printed on Dominion bill
   - Gas: Meter number printed on Columbia Gas bill

6. Record confirmed IDs below:

| Utility | Meter ID | Protocol | Commodity Code | Register Units |
|---|---|---|---|---|
| Electric | TBD | TBD (SCM or SCM+) | TBD | kWh |
| Gas | TBD | TBD (SCM or SCM+) | TBD | ft³ |

---

## Phase 2 — Configure rtlamr2mqtt

**Goal:** Run rtlamr2mqtt as a persistent HAOS add-on, publishing meter readings to MQTT.

### Installation

1. Add the custom repository to HA add-on store (URL above).
2. Install `rtlamr2mqtt`.
3. Configure via the add-on's YAML editor:

```yaml
# rtlamr2mqtt configuration (fill in real meter IDs from Phase 1)
general:
  sleep_for: 0
  verbosity: 5
  tickle_rtlamr: true

mqtt:
  host: core-mosquitto        # Mosquitto add-on hostname on HAOS
  port: 1883
  username: !secret mqtt_user  # add to secrets.yaml if auth enabled
  password: !secret mqtt_password

meters:
  - id: <ELECTRIC_METER_ID>
    protocol: SCM+             # or SCM — match what Phase 1 scan showed
    name: electric_meter
    unit_of_measurement: kWh
    icon: mdi:flash
    device_class: energy
    state_class: total_increasing

  - id: <GAS_METER_ID>
    protocol: SCM+
    name: gas_meter
    unit_of_measurement: ft³
    icon: mdi:fire
    device_class: gas
    state_class: total_increasing
```

4. Start the add-on. It will auto-discover HA via MQTT and create:
   - `sensor.electric_meter` — cumulative kWh (total_increasing)
   - `sensor.gas_meter` — cumulative ft³ (total_increasing)

### Secrets to Add

Add to `/config/secrets.yaml` on HAOS (and local copy):
```yaml
mqtt_user: homeassistant
mqtt_password: <your_mqtt_password>
```

---

## Phase 3 — HA Energy Dashboard Integration

**Goal:** Wire both sensors into the HA Energy dashboard for cost tracking.

### Electric

The electric sensor (`sensor.electric_meter`) reports in kWh with `state_class: total_increasing` and `device_class: energy`. It is directly compatible with the HA Energy dashboard.

Settings → Energy → Electricity grid → Add consumption source → select `sensor.electric_meter`.

The TOU cost tracking is already wired via `sensor.tou_rate` — HA will use the static rate schedule for cost estimation. For true TOU cost tracking, a template energy cost sensor is required (see below).

### Gas

The gas sensor (`sensor.gas_meter`) reports in ft³. HA Energy dashboard accepts gas in ft³ or m³.

Settings → Energy → Gas consumption → Add gas source → select `sensor.gas_meter`.

Enter the gas rate from your Columbia Gas bill (in $/therm or $/ft³ — convert as needed).

### Template Sensors to Add to `configuration.yaml`

```yaml
template:
  - sensor:
      # Convert gas ft³ → therms (for cost calculation)
      # BTU content varies; Columbia Gas VA ~1.020 therms/CCF is typical — confirm from bill
      - name: "Gas Consumption Therms"
        unique_id: gas_consumption_therms
        unit_of_measurement: "therm"
        device_class: gas
        state_class: total_increasing
        state: "{{ (states('sensor.gas_meter') | float(0)) * 0.01020 }}"
```

---

## Phase 4 — Automations

**Goal:** Alert on anomalous consumption; provide daily gas cost summary.

### Automation Ideas

| Automation | Trigger | Action |
|---|---|---|
| Gas leak indicator | Gas consumption increases >5 ft³ in any 5-minute window while HVAC is off | Notify both phones |
| High overnight electric draw | `sensor.electric_meter` rate > X kWh/hr between midnight and 5am | Notify — likely phantom load |
| Monthly gas cost summary | 1st of month at 8am | Notify with prior month total therms + estimated cost |
| Daily gas check | Once daily | Log to logbook — useful for seasonal comparison |

> Gas leak detection via AMR is approximate — AMR reads are every 30–60 sec and meter resolution may be 1 ft³. Use as a secondary indicator, not a primary safety system. Existing ZHA leak sensors are the primary water/gas detection layer.

---

## Dependencies

| Dependency | Status |
|---|---|
| NooElec NESDR SMArt v5 dongle | In hand |
| HAOS USB passthrough | Requires RTL-SDR add-on — not yet installed |
| Mosquitto MQTT broker | Likely installed (used by Wyze MQTT config) — confirm |
| Meter IDs from utility bills | Not yet collected |

---

## Status

| Phase | Status | Notes |
|---|---|---|
| Phase 1 — Passive scan | **Complete** | Meter ID 12868632 confirmed via consumption match + detection frequency |
| Phase 2 — rtlamr2mqtt | **Complete** | Filtering for 12868632 (SCM); `sensor.gas_meter_reading` live in HA |
| Phase 3 — Energy dashboard | **Complete** | `sensor.gas_meter_reading` wired in; utility_meter + CCF template sensors live |
| Phase 4 — Automations | Not started | Waiting on first bill to confirm meter ID + get Columbia Gas rate |

---

## Phase 1 Findings

### Dongle
NooElec NESDR SMArt v5 recognized on Bus 003 Device 012 (`0bda:2838`). rtlamr2mqtt v2026.5.9 installed from custom repository (`6713e36e`). Currently running in `listen_mode: true`.

### Electric Meter — Confirmed AMI
Dominion Energy meter number `0200125181` did not appear in any scan. **The Dominion electric meter is AMI** (advanced mesh network, no continuous 915 MHz broadcast). RTL-SDR cannot read it. Electric monitoring must come from another source (e.g., EM16P clamp meters on the panel).

### Gas Meter — ID Not Yet Confirmed
Physical meter body shows `08 31837167`. That ID has not appeared in scans — `08` is likely a display prefix (commodity/route code), and `31837167` is probably the meter serial, not the ERT endpoint ID. The ERT endpoint ID is on a separate small plastic module attached to the meter body.

16 meters detected in the neighborhood scan. Based on sequential ID patterns, the Columbia Gas meters are almost certainly in the `1286xxxx` / `1287xxxx` cluster:

| ID | Consumption (ft³) |
|---|---|
| 12869308 | 780,810 |
| 12869292 | 172,740 |
| 12868950 | 542,488 |
| 12868818 | 276,552 |
| 12868632 | 860,194 |
| 12869424 | 109,096 |
| 12869676 | 895,748 |
| 12868622 | 171,002 |
| 12868584 | 182,266 |
| 12869506 | 147,224 |

**To identify the correct ID:** Look for a sticker on the small ERT module clipped to the gas meter (separate from the meter body). The 8-digit number there is the broadcast ID. Alternatively, check the Columbia Gas bill for "Meter Number" or "Meter Serial."

---

## Known Unknowns

- Gas meter ERT endpoint ID — check ERT module sticker or Columbia Gas bill
- Actual gas unit/multiplier for Columbia Gas VA — pull from bill (should show therms and CCF)
- Mosquitto auth credentials — check if username/password is configured

---

*Last updated: 2026-05-30*
