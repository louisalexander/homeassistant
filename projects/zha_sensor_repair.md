# ZHA Sensor Re-pair — Office Sink & 2nd Floor Hall Left Sink

## Problem

Two Third Reality 3RWS18BZ leak sensors fire frequent `attribute_updated` logbook events
(attribute `on_off`, value 0) every ~60 seconds, while all other identical sensors do not.
Root cause: sensors were paired under an older ZHA quirk version that configured a shorter
On/Off cluster reporting interval. They also lack LQI/RSSI entities that all other 3RWS18BZ
sensors expose — further evidence of a stale quirk at pair time.

**Affected devices:**
- `binary_sensor.office_sink_leak_detector` — Office Sink Leak Detector
- `binary_sensor.second_floor_hall_bathroom_left_sink` — Second Floor Hall Bathroom Left Sink

## Fix

Re-pair both sensors. ZHA will apply the current quirk version, normalize the reporting
interval, and expose the missing LQI/RSSI entities.

## Re-pair Steps (per sensor)

1. In HA: Settings → Integrations → Zigbee Home Automation → devices → find the sensor
2. Click **Reconfigure** (or Remove Device, then re-add)
3. Put the sensor into pairing mode: press and hold the reset button on the back ~5 seconds
   until the LED blinks rapidly
4. HA should discover it and re-pair under the current quirk
5. Rename the device back to its original friendly name if HA resets it
6. Verify LQI/RSSI entities now appear on the device page

## After Re-pairing

- Confirm `attribute_updated` logbook noise stops for both sensors
- Confirm LQI and RSSI sensor entities are now present (matching other sensors)
- No automation or dashboard changes needed — entity IDs are preserved on re-pair

## Notes

- Re-pair one at a time; confirm the first is working before doing the second
- Sensors must be within reasonable range of a Zigbee router/coordinator during pairing
- The automations, HomeKit bridge, and dashboard all reference these entity IDs —
  re-pairing preserves entity IDs so no config changes are needed afterward

*Last updated: 2026-05-23*
*Status: Not yet done*
