# Garage Doors

Both garage doors are monitored by Home Assistant through the ESPHome openers. You'll get notifications for opens, closes, and situations that need attention.

---

## Notifications You'll Receive

### Every Open and Close

Both phones get a push notification whenever either door opens or closes — whether you did it yourself, someone else did, or it happened unexpectedly.

### If a Door Is Open When Everyone Leaves

If either door is open and both of you are away from home, you'll get a notification with a **Close It** button. Tap it to close the door remotely without opening the app.

### If a Door Is Left Open for 30 Minutes

If a door has been open for 30+ minutes, both phones get a reminder notification with a **Close It** button.

---

## Closing Remotely

You can close either door from anywhere:

- **From a notification** — tap the **Close It** button that appears in the relevant alert
- **From the Home Command dashboard** — Security tab has buttons for both doors
- **From Alexa** — "Alexa, close the garage" (triggers the garage script)

---

## A Note on Door Names

The garage door opener installer wired the two doors in reverse — what the system internally calls "left" is physically the right door, and vice versa. The **labels in the app and dashboard reflect the physical door** (left means the left door when you're standing inside the garage looking out), so what you see in the UI is correct. This is just a behind-the-scenes quirk.

---

## What's Shown on the Dashboard

The Security tab shows the current state of each door (open or closed) and buttons to open or close either one. You can also see the last activity time for both.
