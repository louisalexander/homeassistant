# Entity ID Reference

Quick-lookup table of all key entity IDs. Use these in automations, templates, and dashboard cards.

---

## Climate

| Entity | Description |
|---|---|
| `climate.bedroom` | Ecobee — master bedroom (2nd floor) |
| `climate.office` | Ecobee — office (3rd floor) |
| `sensor.bedroom_temperature` | Master bedroom temp (Ecobee built-in) |
| `sensor.bedroom_1_temperature` | Bedroom 1 temp (SmartSensor) |
| `sensor.bedroom_2_temperature` | Bedroom 2 temp (SmartSensor) |
| `sensor.bedroom_3_temperature` | Bedroom 3 temp (SmartSensor) |
| `sensor.bedroom_4_temperature` | Bedroom 4 temp (SmartSensor) |
| `sensor.office_temperature` | Office temp (Ecobee built-in) |
| `sensor.tou_period` | Current TOU period: `peak` / `off_peak` / `super_off_peak` |
| `sensor.tou_rate` | Current TOU rate in $/kWh |
| `input_boolean.peak_mode` | True during TOU peak hours |

---

## Garage Doors

| Entity | Description |
|---|---|
| `cover.left_garage_door_opener_garage_door` | Left garage door |
| `cover.right_garage_door_opener_garage_door` | Right garage door |

---

## Cameras

| Entity | Description |
|---|---|
| `camera.garage_stickup_cam_1_live_view` | Ring Indoor Cam — garage |
| `camera.personnel_door_live_view` | Ring Indoor Cam — personnel door |
| `camera.wyze_cam_v3_1` | Wyze v3 — office (local RTSP) |
| `sensor.garage_stickup_cam_1_last_activity` | Ring — last motion/activity timestamp |
| `sensor.personnel_door_last_activity` | Ring — last motion/activity timestamp |

---

## Chimes / Sirens

| Entity | Description |
|---|---|
| `siren.hallway_siren` | Ring Chime V3 — 2nd floor hallway |
| `siren.master_bedroom_siren` | Ring Chime V3 — master bedroom area |
| `siren.office_siren` | Ring Chime V3 — 3rd floor office |
| `siren.garage_siren` | Ring Chime V3 — garage |

---

## Leak Sensors

| Entity | Location |
|---|---|
| `binary_sensor.kitchen_sink_leak_detector` | Kitchen sink |
| `binary_sensor.laundry_room_leak_sensor` | Laundry room |
| `binary_sensor.master_bathroom_left_sink_leak_detector` | Master bath left sink |
| `binary_sensor.master_bathroom_right_sink_leak_detector` | Master bath right sink |
| `binary_sensor.powder_room_leak_sensor` | Powder room |
| `binary_sensor.office_sink_leak_detector` | Office sink |
| `binary_sensor.bedroom_3_sink_leak_detector` | Bedroom 3 sink |

---

## Presence

| Entity | Description |
|---|---|
| `zone.home` | Count of people in Home zone — use this for presence automations |
| `person.louis` | Louis's location |
| `person.lindsay` | Lindsay's location |

---

## Energy / Power

| Entity | Description |
|---|---|
| `sensor.front_room_plug_1_power` | Front room plug — live watts |
| `sensor.upstairs_hallway_1_power` | Upstairs hallway plug 1 — live watts |
| `sensor.upstairs_hallway_plug_2_power` | Upstairs hallway plug 2 — live watts |
| `sensor.office_plug_1_power` | Office plug — live watts |
| `sensor.front_room_plug_1_summation_delivered` | Front room plug — cumulative kWh |

---

## Air Quality

| Entity | Description |
|---|---|
| `sensor.bedroom_carbon_dioxide` | Bedroom CO₂ (ppm) |
| `sensor.bedroom_volatile_organic_compounds` | Bedroom VOC (ppb) |
| `sensor.bedroom_air_quality_index` | Bedroom AQI |
| `sensor.bedroom_humidity` | Bedroom humidity (%) |

---

## Notifications

| Entity | Description |
|---|---|
| `notify.mobile_app_louis_iphone` | Push to Louis's iPhone |
| `notify.mobile_app_lindsays_iphone` | Push to Lindsay's iPhone |
| `media_player.master_bedroom_sonos_one` | Sonos One — master bedroom (TTS) |
| `media_player.sonos_play_5` | Sonos Play:5 (TTS) |

---

## Mock Door Sensors (Konnected placeholders)

| Entity | Simulates |
|---|---|
| `input_button.front_door_test` | Front door open |
| `input_button.garage_door_test` | Garage door open |
| `input_button.rear_door_test` | Rear door open |

Replace these trigger entities with real Konnected `binary_sensor` entities when the panel is wired.

---

## Notification URL Sensors

| Entity | Description |
|---|---|
| `sensor.nabu_casa_security_url` | Deep link to Security dashboard tab |
| `sensor.nabu_casa_climate_url` | Deep link to Climate dashboard tab |
