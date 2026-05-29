# Climate & Comfort

## The Thermostats

There are two active Ecobee thermostats in the house. A third is on hand and will be installed on the first floor soon.

| Thermostat | Controls |
|---|---|
| **Master Bedroom** (2nd floor) | Bedroom wing — master bed, bedrooms 1–4 |
| **Office** (3rd floor) | Office zone |
| 1st Floor *(coming soon)* | Main living areas — kitchen, living room, dining room |

Each thermostat runs in **auto mode** — it heats and cools as needed to keep the house in the target range.

---

## What Happens Automatically

### When Everyone Leaves

As soon as both phones leave the home area, both thermostats switch to **Away mode**. The setpoints relax to save energy — the house won't heat or cool as aggressively while empty.

### When Someone Gets Home

As soon as the first phone enters the home area, both thermostats switch back to **Home mode** and return to normal comfort setpoints.

You don't need to do anything — this is fully automatic based on phone location.

---

## Energy Saving During Peak Hours

Dominion Energy charges significantly more per kilowatt-hour during "peak" hours. The Ecobee Eco+ TOU feature knows the utility's schedule and uses the house's thermal mass to save money automatically.

**What it does:**

- **Before peak hours start:** Pre-cools the house lower than normal (summer) or pre-heats it higher than normal (winter). This stores "cold" or "heat" in the walls and furniture.
- **During peak hours:** The AC/heat runs as little as possible — the house coasts on what was stored. Your electricity bill is lower.
- **After peak hours:** Returns to normal setpoints.

**You'll notice** the thermostat setpoints looking slightly different than usual around peak times — that's normal and intentional.

**Peak hours (Dominion Energy Schedule 1G):**

| Season | Peak Hours | Days |
|---|---|---|
| Summer (May–Sep) | 3:00 PM – 6:00 PM | Weekdays only |
| Winter (Oct–Apr) | 6:00 AM – 9:00 AM and 5:00 PM – 8:00 PM | Weekdays only |

Weekends have no peak period — just standard and off-peak rates.

---

## Room Sensors

Each Ecobee has SmartSensors placed in individual bedrooms. These measure the temperature in each room independently, so the thermostat can balance comfort across multiple rooms — not just where the thermostat itself is mounted.

Active sensors: master bedroom, bedrooms 1, 2, 3, 4, and the office.

---

## Controlling the Thermostats

You can adjust temperature from anywhere:

- **Ecobee app** — full control, schedule management, all settings
- **Apple Home** — both thermostats appear; Siri works ("Hey Siri, set the bedroom to 72")
- **Alexa** — "Alexa, set the bedroom thermostat to 72"
- **HA dashboard** — Climate tab has full thermostat controls

Any manual adjustment you make overrides the schedule temporarily (a "hold") and returns to the schedule automatically after the hold period ends.

---

## Air Quality

The master bedroom has sensors that monitor:

- **CO₂** (carbon dioxide) — rises when the space is occupied and ventilation is limited
- **VOCs** (volatile organic compounds) — can spike from cleaning products, cooking, or off-gassing
- **Humidity**
- **Air Quality Index (AQI)**

If CO₂ or VOC levels reach unhealthy thresholds, you'll get a push notification. See [Notifications](notifications.md) for details.

---

## Bedroom Air Purifier

A **Winix air purifier** in the master bedroom runs automatically and integrates with the air quality sensors.

### Automatic VOC Response

When bedroom VOCs rise above 500 µg/m³ (Grade C on the Health tab) for more than 2 minutes, the purifier automatically switches to **Manual mode at high speed** to clear the air faster than its built-in Auto mode would. Once VOCs have been below 250 µg/m³ for 10 minutes, it returns to Auto.

### PlasmaWave Schedule

The purifier's **PlasmaWave** ionizer feature breaks down bacteria, odors, and chemical vapors but can produce trace ozone — not ideal in a closed bedroom overnight. It turns off automatically at **10 PM** and back on at **7 AM**.

The VOC boost automation respects this schedule: if it kicks in overnight while PlasmaWave is off, it will not re-enable it.

### Dashboard

The purifier tile on the Climate tab shows the current mode and airflow level. The icon color indicates status:

| Color | Meaning |
|---|---|
| Green | Auto mode — normal operation |
| Orange | Manual · High — boosted by VOC automation |
| Yellow | Manual at lower speed |
| Blue | Sleep mode |
| Grey | Off |
