# Home Assistant Configuration

Personal Home Assistant OS configuration for a 3-floor house. Address and location details are stored in `secrets.yaml` (gitignored). Runs on HAOS (local network), remotely accessible via Nabu Casa.

## What's in this repo

| File / Dir | Purpose |
|---|---|
| `configuration.yaml` | Main HA config — includes, TOU sensors, `input_boolean.peak_mode` |
| `automations.yaml` | All active automations (single source of truth — 26 automations) |
| `homekit.yaml` | HomeKit bridge entity whitelist |
| `influxdb_ha.yaml` | InfluxDB domain/entity include-exclude list |
| `scripts.yaml` | Alexa-invocable garage scripts |
| `scenes.yaml` | Scenes (currently empty) |
| `dashboard.yaml` | Reference copy of the Lovelace dashboard (not the live file) |
| `blueprints/` | HA default automation/script/template blueprints |
| `secrets.yaml.template` | Template listing every required secret key — copy to `secrets.yaml` and fill in |
| `PLAN.md` | Master execution plan across all active smart home projects |
| `projects/` | Detailed design docs for each project |

## Setup on a fresh install

1. Copy config files to `/config/` on HAOS
2. Copy `secrets.yaml.template` → `secrets.yaml`, fill in real values
3. Restart HA: `ha core restart`

## Active projects

See [`PLAN.md`](PLAN.md) for current status and priorities. Detailed docs in [`projects/`](projects/):

- **Camera system** — Ring (perimeter/cloud) + Wyze post-Thingino (interior/local RTSP)
- **Alarm system** — Konnected Alarm Panel Pro → Alarmo → Ring Alarm Base Station → Ring Protect Pro
- **Lighting** — Whole-house Inovelli Blue Series Zigbee switch rollout (29× dimmers/on-off, 3× fan+light)
- **Climate** — Two Ecobee thermostats; Eco+ TOU integration; 1st floor unit pending install
- **Energy monitoring** — EM16P dual-panel energy monitors; TOU rate sensors live
- **Smoke detectors** — 9× First Alert Z-Wave ZCOMBO-G replacement
- **Smart locks** — Schlage BE469ZP (front door first)

## Integrations

ZHA (Zigbee, channel 25) · Z-Wave · ESPHome · Ring · Ecobee · Sonos · InfluxDB · HomeKit Bridge · Alexa · Nabu Casa

## Secrets

`secrets.yaml` is gitignored. All secret values live only on the HAOS box and locally. Use `secrets.yaml.template` to reconstruct. Nabu Casa URLs in `automations.yaml` are referenced via `!secret nabu_casa_security_url` and `!secret nabu_casa_climate_url`.
