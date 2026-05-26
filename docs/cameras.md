# Cameras

## Where the Cameras Are

| Camera | Location | What It Covers |
|---|---|---|
| **Garage Cam** | Inside the garage | Garage interior, both doors visible |
| **Personnel Door Cam** | Inside, at the side door (house side) | The door between garage and house |
| **Office Cam** | 3rd floor office | Office interior — always-on local stream |

**Coming soon:** Ring Doorbell at the front door, plus two outdoor cameras (driveway and rear of house).

---

## Motion Alerts — Chimes

When the Garage Cam or Personnel Door Cam detects motion, Ring rings the chimes directly — this is handled natively by Ring, not by Home Assistant. Motion-to-chime is configured per-camera in the Ring app under **Motion Alerts** and per-chime under **Linked Devices**.

| Chime | Location |
|---|---|
| 1st floor | Kitchen area |
| 2nd floor | Hallway |
| 2nd floor | Master bedroom area |
| 3rd floor | Office |

Ring's native chime-on-motion fires in 1–3 seconds. Cooldown and alert frequency are configured in the Ring app per device.

---

## Watching the Cameras

### From the Dashboard — Security Tab

The Security tab in the Home Command dashboard shows all three camera feeds:

- **Garage Cam** and **Personnel Door Cam** — show a snapshot that refreshes every 2 minutes and also updates immediately when motion is detected. Tap the image to open a live stream.
- **Office Cam** — always shows a live stream (the Wyze camera streams locally over your home network with no cloud involved).

### From the Ring App

Open the Ring app to watch live footage from the Ring cameras, review recorded clips, or check motion history. Ring Protect Pro covers all Ring cameras with video history.

### From Apple Home

All three cameras appear in the Home app via the HA bridge (**Garage Camera**, **Personnel Door Camera**, **Office Camera**). Tap a camera tile to start a live stream.

Ring cameras stream via the cloud — expect 3–5 seconds of startup latency. The Office Camera (Wyze/Thingino) streams locally and should be nearly instant.

---

## Ring Subscription

Ring Protect Pro is active on an annual subscription. This covers:
- Video recording and cloud history for all Ring cameras
- Professional monitoring (when the alarm system is installed)

---

## What's Coming

| Device | Where | Status |
|---|---|---|
| Ring Video Doorbell Powered 2K | Front door | Hardware in hand — not yet installed |
| Ring Outdoor Cam + Solar Panel × 2 | Driveway + rear | Hardware in hand — sun exposure walk needed before mounting |
