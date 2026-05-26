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
