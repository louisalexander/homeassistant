# Hardware Inventory

Complete inventory of every device in the system.

---

## Hubs & Infrastructure

| Device | Location | Status | Notes |
|---|---|---|---|
| Mini S13 PC | SMC (structured media cabinet) | Active | Runs HAOS |
| ZBT-2 Zigbee USB dongle | SMC | Active | On Sabrent hub + extension cable; channel 25 |
| Z-Wave USB dongle | SMC | Active | Lightly used; reserved for locks + smoke detectors |

---

## Climate

| Device | Location | Entity | Status |
|---|---|---|---|
| Ecobee Premium thermostat | Master bedroom (2nd floor) | `climate.bedroom` | Active |
| Ecobee Premium thermostat | Office (3rd floor) | `climate.office` | Active |
| Ecobee Premium thermostat | 1st floor | TBD | **Purchased — not installed** |
| Trane TEM4A0B24S21SBA air handler | Attic | — | Serves 2nd + 3rd floor zones |
| York PS9B12N080DH11B furnace | Crawlspace | — | 1st floor zone; 80k BTU, 2005 vintage |

### Ecobee Remote Sensors

| Sensor | Location | Entity |
|---|---|---|
| SmartSensor | Bedroom 1 | `sensor.bedroom_1_temperature` |
| SmartSensor | Bedroom 2 | `sensor.bedroom_2_temperature` |
| SmartSensor | Bedroom 3 | `sensor.bedroom_3_temperature` |
| SmartSensor | Bedroom 4 | `sensor.bedroom_4_temperature` |
| Built-in (master bed Ecobee) | Master bedroom | `sensor.bedroom_temperature` |
| Built-in (office Ecobee) | Office | `sensor.office_temperature` |

---

## Cameras

| Device | Qty | Location | Status | Entity |
|---|---|---|---|---|
| Ring Indoor Cam (plug-in) | 1 | Garage interior | Active | `camera.garage_stickup_cam_1_live_view` |
| Ring Indoor Cam (plug-in) | 1 | Personnel door (house side) | Active | `camera.personnel_door_live_view` |
| Ring Outdoor Cam + Solar Panel | 2 | Pending — sun exposure walk first | Not mounted | — |
| Ring Video Doorbell Powered 2K | 1 | Front door | Not installed | — |
| Ring Doorbell Transformer | 1 | In hand | Not installed | — |
| Wyze Cam v3 (Thingino firmware) | 1 | Office (3rd floor) | Active — RTSP stream live | `camera.wyze_cam_v3_1` |

---

## Ring Chimes

| Device | Location | Entity |
|---|---|---|
| Ring Chime V3 | 1st floor kitchen | `siren.kitchen_chime` |
| Ring Chime V3 | 1st floor foyer | `siren.foyer_chime` |
| Ring Chime V3 | 2nd floor hallway | `siren.hallway_siren` |
| Ring Chime V3 | 3rd floor office | `siren.office_siren` |

!!! note
    Ring Chimes V3 expose only 2 tones: `ding` and `motion`. Volume is controlled via `number.[name]_volume` (0–11).

---

## Leak Sensors

| Location | Entity |
|---|---|
| Kitchen sink | `binary_sensor.kitchen_sink_leak_detector` |
| Laundry room | `binary_sensor.laundry_room_leak_sensor` |
| Master bathroom left sink | `binary_sensor.master_bathroom_left_sink_leak_detector` |
| Master bathroom right sink | `binary_sensor.master_bathroom_right_sink_leak_detector` |
| Powder room | `binary_sensor.powder_room_leak_sensor` |
| Office sink | `binary_sensor.office_sink_leak_detector` |
| Bedroom 3 sink | `binary_sensor.bedroom_3_sink_leak_detector` |

2× additional leak sensors on hand — pair to ZHA; existing leak automations pick them up automatically.

---

## Smart Plugs (Power Monitoring)

| Location | Power entity | kWh entity |
|---|---|---|
| Front room | `sensor.front_room_plug_1_power` | `sensor.front_room_plug_1_summation_delivered` |
| Upstairs hallway | `sensor.upstairs_hallway_1_power` | `sensor.upstairs_hallway_1_summation_delivered` |
| Upstairs hallway #2 | `sensor.upstairs_hallway_plug_2_power` | `sensor.upstairs_hallway_plug_2_summation_delivered` |
| Office | `sensor.office_plug_1_power` | `sensor.office_plug_1_summation_delivered` |

---

## Audio

| Device | Location | Entity |
|---|---|---|
| Sonos One | Master bedroom | `media_player.master_bedroom_sonos_one` |
| Sonos Play:5 | Living area | `media_player.sonos_play_5` |
| Echo (various) | Throughout house | Multiple `media_player.` entities |

---

## Lighting — Inovelli Blue Series (Zigbee, ZHA)

See [Lighting](lighting.md) for the full rollout plan. Hardware on hand:

| Item | Qty | Status |
|---|---|---|
| VZW31-SN Blue Series 2-1 (outdoor batch) | 10 | **In hand — ready to install** |
| VZW05 Aux switch (outdoor batch) | 5 | **In hand — ready to install** |
| VZW31-SN (indoor batches) | 22 remaining to order | Order one batch at a time |
| VZW36 Fan+Light controller | 3 | To order with fan batches |
| VZM35-SN Fan Canopy Module | 3 | To order with fan batches |
| VZW05 Aux (indoor batches) | 13 remaining | Order with each batch |

---

## Security / Alarm

| Device | Status | Notes |
|---|---|---|
| Konnected Alarm Panel Pro | **Purchased — not installed** | 4 zones; replaces GE Concord Express 60-806 |
| GE Concord Express 60-806 | Existing (being replaced) | 4 wired zones wired |
| Schlage BE469ZP Z-Wave lock | Not yet ordered | Front door first |
| First Alert ZCOMBO-G smoke/CO | Not yet ordered | 9× needed; order when ready to install same week |
| Ring Alarm Gen 2 Base Station | Not yet ordered | Bridges Alarmo → Ring Pro monitoring via ring-mqtt |

---

## Network

| Device | Location | Role |
|---|---|---|
| Eero Pro (mesh) | Throughout house | WiFi gateway — 1 Gbps fiber |
| TP-Link TL-SG116 16-port switch | SMC | LAN backbone |
| VCELINK 24-port Cat6A keystone patch panel | SMC | Structured wiring |
| ScreenBeam MoCA adapters | Per floor | Ethernet over coax between floors |

---

## Hardware On Hold / Deferred

| Device | Project | Reason |
|---|---|---|
| Gosund SL2 LED strip | `gosund_sl2.md` | Can't locate device |
| Govee Floor Lamp 2 | — | Power plug misplaced |
