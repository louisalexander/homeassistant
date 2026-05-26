# Notifications & Alerts

All notifications go to both iPhones unless noted otherwise. Here's every alert you might receive, what it means, and what to do.

---

## Emergency Alerts

These bypass silent mode and Do Not Disturb — they will sound no matter what.

### Water Leak Detected

> **"Water detected — [Room name]"**

A leak sensor found water. Go check immediately.

- The room name in the alert tells you exactly where to look
- Turn off the water supply to that fixture if there's an active leak
- The alert clears once the sensor dries out

See [Leak Detection](leaks.md) for sensor locations.

---

## Garage Door Alerts

### Door Opened or Closed

> **"Garage — Left door opened"** / **"Garage — Right door closed"**

Normal notification whenever either door changes state. No action needed unless unexpected.

### Garage Open While Away

> **"Garage door is open and you're both away"**  
> Action button: **Close It**

Sent when a door is open and both phones are away from home. Tap **Close It** to close the door remotely.

### Garage Left Open Too Long

> **"Garage door has been open for 30 minutes"**  
> Action button: **Close It**

Sent if either door stays open for 30 continuous minutes. Tap **Close It** or check manually.

---

## Air Quality Alerts

### CO₂ High — Bedroom

> **"Bedroom CO₂ is elevated"**

Carbon dioxide in the master bedroom has reached an unhealthy level. Open a window or run the HVAC fan to bring fresh air in.

### VOC High — Bedroom or Office

> **"Bedroom VOC levels are elevated"** / **"Office VOC levels are elevated"**

Volatile organic compounds are high. Common causes: cleaning products recently used, fresh paint, cooking fumes distributing through the HVAC. Ventilate the space.

---

## System Alerts

### Low Battery

> **"[Device name] battery is low"**

One of the ~15 Zigbee sensors (leak detectors, SmartSensors, etc.) needs a battery. Replace when convenient — the device will still work until the battery is fully depleted.

---

## Physical Chimes (Not Phone Notifications)

### Motion at Garage or Personnel Door Camera

When either Ring camera detects motion, Ring rings the chimes directly. This is handled natively by Ring — not by Home Assistant. Cooldown and frequency are configured per-device in the Ring app.

---

## Notification Deep Links

Most notifications include a link that opens directly to the relevant dashboard tab when tapped. Garage and security alerts open the Security tab; climate-related alerts open the Climate tab.
