# Grafana Analytics Setup

InfluxDB version: **1.x (Chronograf UI)**

---

## Step 1 — InfluxDB: Create Database & User ✅

In Chronograf (InfluxDB Web UI):

1. Click **crown icon** (Admin) → **InfluxDB** tab → **Create Database**
   - Name: `homeassistant`
2. Click **Users** tab → **Create User**
   - Username: `homeassistant`
   - Grant access to `homeassistant` database

---

## Step 2 — Wire HA to InfluxDB

**In `secrets.yaml`** (HA config root), add:
```yaml
influxdb_password: <your-influxdb-password>
```

**In `configuration.yaml`** (HA config root), add:
```yaml
influxdb: !include influxdb_ha.yaml
```

**Copy `influxdb_ha.yaml`** from `/homeassistant/influxdb_ha.yaml` into `/config/influxdb_ha.yaml`
(accessible via Studio Code Server add-on or Samba share).

**Restart Home Assistant** → Settings → System → Restart

Wait ~5 minutes for data to start flowing into InfluxDB.

**Verify data is arriving:**
In Chronograf → Data Explorer → select `homeassistant` database → you should see measurements appearing.

---

## Step 3 — Connect Grafana to InfluxDB

1. Open Grafana (HA → Add-ons → Grafana → Open Web UI)
2. Go to **Connections → Data Sources → Add new data source**
3. Select **InfluxDB**
4. Set:
   - Query Language: **InfluxQL** (NOT Flux — that's for v2)
   - URL: `http://localhost:8086`
   - Database: `homeassistant`
   - User: `homeassistant`
   - Password: *(your InfluxDB password)*
5. Click **Save & Test** — should show green ✓

---

## Step 4 — Build the Dashboard

1. Grafana → **Dashboards → New Dashboard → Add visualization**
2. Select the InfluxDB data source
3. Use InfluxQL queries (see below — different from the Flux queries in grafana_queries.md)

### InfluxQL Queries for each panel:

**Temperature by Zone (Time series)**
```sql
SELECT mean("value") FROM "°F"
WHERE ("entity_id" =~ /bedroom_temperature|upstairs_temperature|dad_s_new_echo_temperature/)
AND $timeFilter
GROUP BY time($__interval), "entity_id" fill(none)
```

**CO₂ (Time series)**
```sql
SELECT mean("value") FROM "ppm"
WHERE "entity_id" = 'sensor.bedroom_carbon_dioxide'
AND $timeFilter
GROUP BY time($__interval) fill(none)
```

**VOCs (Time series)**
```sql
SELECT mean("value") FROM "ppb"
WHERE "entity_id" = 'sensor.bedroom_volatile_organic_compounds'
AND $timeFilter
GROUP BY time($__interval) fill(none)
```

**Humidity (Time series)**
```sql
SELECT mean("value") FROM "%"
WHERE "entity_id" = 'sensor.bedroom_humidity'
AND $timeFilter
GROUP BY time($__interval) fill(none)
```

**Live Power Draw per Plug (Time series)**
```sql
SELECT mean("value") FROM "W"
WHERE ("entity_id" =~ /front_room_plug_1_power|upstairs_hallway_1_power|upstairs_hallway_plug_2_power|office_plug_1_power/)
AND $timeFilter
GROUP BY time($__interval), "entity_id" fill(none)
```

**Garage Door Events (State timeline)**
```sql
SELECT "value" FROM "state"
WHERE ("entity_id" =~ /left_garage_door|right_garage_door/)
AND $timeFilter
```

**Presence — Who's Home (State timeline)**
```sql
SELECT "value" FROM "state"
WHERE ("entity_id" =~ /person.louis|person.lindsay/)
AND $timeFilter
```

**Leak Sensors (State timeline)**
```sql
SELECT "value" FROM "state"
WHERE ("entity_id" =~ /leak_detector|leak_sensor|butlers_kitchen_sink|second_floor_hall/)
AND $timeFilter
```

---

## Step 5 — Panel types to use

| Data | Grafana panel type |
|---|---|
| Temperature, CO₂, VOC, humidity, power | Time series |
| kWh totals | Stat |
| Garage doors, presence, leaks | State timeline |

---

## Step 6 — Polish

- Set dashboard refresh to `30s`
- Set default time range to `Last 24 hours`
- Star the dashboard so it appears on Grafana home
- Optional: add Grafana as an iframe card in your Lovelace dashboard
