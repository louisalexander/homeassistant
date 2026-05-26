# Setup Guide

How to stand up this configuration on a fresh HAOS install.

---

## Prerequisites

- Home Assistant OS running (HAOS recommended for full add-on support)
- SSH access enabled (Settings → Add-ons → Terminal & SSH)
- All hardware connected (Zigbee dongle, Z-Wave dongle, etc.)

---

## Step 1 — Clone the Repo

On your local machine:

```bash
git clone https://github.com/louisalexander/homeassistant.git
cd homeassistant
```

---

## Step 2 — Create secrets.yaml

```bash
cp secrets.yaml.template secrets.yaml
```

Edit `secrets.yaml` and fill in all values. Required keys:

| Key | Description |
|---|---|
| `nabu_casa_security_url` | Your Nabu Casa URL + `/lovelace/security` |
| `nabu_casa_climate_url` | Your Nabu Casa URL + `/lovelace/climate` |
| `influxdb_password` | InfluxDB password you set up |
| `home_street` | Your street address |
| `home_city` | City |
| `home_state` | State |
| `home_zip` | ZIP code |

---

## Step 3 — Push Config Files to HAOS

```bash
# Replace 192.168.x.x with your HA instance's local IP
HA=root@192.168.x.x

scp configuration.yaml $HA:/config/configuration.yaml
scp configuration_tou_addition.yaml $HA:/config/configuration_tou_addition.yaml
scp automations.yaml $HA:/config/automations.yaml
scp scripts.yaml $HA:/config/scripts.yaml
scp homekit.yaml $HA:/config/homekit.yaml
scp influxdb_ha.yaml $HA:/config/influxdb_ha.yaml
scp secrets.yaml $HA:/config/secrets.yaml

ssh $HA "ha core restart"
```

---

## Step 4 — Install Required Integrations

Install these via HA Settings → Integrations:

- **Ring** — cameras, chimes, doorbell
- **Ecobee** — thermostats
- **Sonos** — speakers
- **Mobile App** — on each iPhone
- **ESPHome** — garage door openers
- **InfluxDB** — time-series storage

---

## Step 5 — Install Required Add-ons

Settings → Add-ons:

- **Mosquitto broker** — MQTT for Thingino camera
- **InfluxDB** — time-series database
- **Grafana** — analytics dashboards
- **Studio Code Server** — browser-based file editor
- **Terminal & SSH** — SSH access
- **File editor** — simple file editing

---

## Step 6 — Install Custom Dashboard Components (HACS)

Install HACS, then install these integrations:

- `button-card` — used extensively on Climate tab
- `mushroom` — thermostat controls, chips
- `apexcharts-card` — temperature chart

---

## Step 7 — Restore the Dashboard

The live dashboard is stored in HA's `.storage/` directory, not as a YAML file:

```bash
# The reference copy in the repo is a starting point
# Push it to the live location
scp dashboard.yaml root@192.168.x.x:/config/.storage/lovelace.home_command_dashboard
ssh root@192.168.x.x "ha core restart"
```

---

## Step 8 — Configure Zigbee (ZHA)

1. Plug in ZBT-2 USB dongle (via USB hub + extension cable — keep away from WiFi radio)
2. Settings → Integrations → Add → Zigbee Home Automation
3. Select the ZBT-2 device
4. Channel: **25**
5. Pair devices in order: mains-powered first (switches, plugs), then battery sensors

---

## Day-to-Day Workflow

### Update automations

```bash
scp automations.yaml root@192.168.x.x:/config/automations.yaml
# Then: Developer Tools → YAML → Reload Automations (no restart needed)
```

### Update dashboard

```bash
# Always pull fresh first
ssh root@192.168.x.x "cat /config/.storage/lovelace.home_command_dashboard" > /tmp/lovelace_dashboard.json
# Edit /tmp/lovelace_dashboard.json
scp /tmp/lovelace_dashboard.json root@192.168.x.x:/config/.storage/lovelace.home_command_dashboard
ssh root@192.168.x.x "ha core restart"
```

### Update other config files

```bash
scp configuration.yaml root@192.168.x.x:/config/configuration.yaml
ssh root@192.168.x.x "ha core restart"
```

---

## Secrets Backup

`secrets.yaml` is gitignored and lives only on the HAOS box and your local machine. Back it up separately — it cannot be reconstructed from the repo (only the template can).

```bash
# Pull from HAOS to local backup
scp root@192.168.x.x:/config/secrets.yaml ~/secrets_backup.yaml
```
