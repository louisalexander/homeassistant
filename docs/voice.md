# Voice Control

## Alexa

Alexa is the primary voice interface. There are Echo devices throughout the house. You can control most smart home devices by asking any of them.

### Thermostats

Alexa has direct access to the Ecobee thermostats:

| What to say | What it does |
|---|---|
| "Alexa, set the bedroom to 72" | Sets the master bedroom thermostat to 72°F |
| "Alexa, set the office to 70" | Sets the office thermostat to 70°F |
| "Alexa, what's the temperature in the bedroom?" | Reads the current bedroom temperature |
| "Alexa, make it warmer/cooler" | Adjusts setpoint by a few degrees |

### Garage Doors

| What to say | What it does |
|---|---|
| "Alexa, open the garage" | Opens both garage doors |
| "Alexa, close the garage" | Closes both garage doors |

### Smart Plugs

The smart plugs throughout the house are available to Alexa by room name. Ask Alexa to turn them on or off by device name as it appears in the Alexa app.

---

## Apple Home & Siri

The following devices are available in the Apple Home app and work with Siri:

| What's in Apple Home | Notes |
|---|---|
| Both Ecobee thermostats | Paired directly to Ecobee — full control |
| Garage doors (×2) | Open/close, status |
| Leak sensors (×11) | Status visible — alerts not routed through Home |
| Smart plugs (×4) | On/off control |
| Garage Camera | Via HA bridge — 3–5s stream startup (Ring cloud) |
| Personnel Door Camera | Via HA bridge — 3–5s stream startup (Ring cloud) |
| Office Camera | Via HA bridge — local RTSP, fast |

Siri examples:
- "Hey Siri, what's the temperature in the bedroom?"
- "Hey Siri, set the office thermostat to 68"
- "Hey Siri, is the garage closed?"

### Home & Away Automations in Apple Home

Ecobee's native presence detection within Apple Home is separate from the Home Assistant presence automation. Both can run concurrently — the Home Assistant automation uses your phone's GPS zone, while Ecobee can use its own SmartSensor occupancy detection. If you want only one to run, use the Ecobee app to disable its built-in away automation.

---

## Home Command Dashboard

The full dashboard is accessible from anywhere via the Nabu Casa cloud link (saved in the Home app shortcut on both phones). The dashboard has tabs for:

- **Security** — camera feeds, garage door controls, door sensors
- **Climate** — thermostat controls, room temperatures, TOU status
