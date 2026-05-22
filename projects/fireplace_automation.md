# Gas Fireplace Automation

## Hardware Description

- **Gas valve:** Standard wall toggle switch — **confirmed millivolt** (thin brown wire at switch terminals). Cannot use a standard smart switch — requires a dry-contact relay that closes the circuit without carrying load.
- **Blower:** Separate rotary dial — **clicks to turn on**, then continuously variable speed. This is a standard fireplace blower speed controller with integrated power switch (typically 120V triac or rheostat type). The click is the on/off engagement point; past it the speed varies continuously.

---

## Investigation Required First

Before choosing hardware, answer these questions at the fireplace:

- [ ] **Fireplace make/model** — check the data plate inside the unit (usually on a side panel or behind the lower louver)
- [ ] **On/off switch type** — open the wall plate and check:
  - If wires are thin (18–22 AWG), it's likely **millivolt** (low-voltage thermostat circuit, ~750mV)
  - If wires are standard household wire (12–14 AWG), it's **120V**
  - Millivolt systems cannot use standard smart switches — need a dry-contact relay
- [ ] **Does the fireplace have an existing RF remote?** Check the original manuals or look for a remote receiver inside the firebox area
- [ ] **Blower dial location** — is it the same wall plate as the on/off switch, or separate?
- [ ] **Blower motor wattage** — check label on the blower motor itself (access via lower louver panel); typically 25–50W for residential fireplace blowers

---

## Automation Options

### Option A — Bond Bridge (easiest, if RF remote exists or can be added)

Many gas fireplaces support aftermarket RF remote kits (e.g., Skytech 1001-A or similar). If an RF remote is added:
- **Bond Bridge** (~$99) learns the RF signals and exposes the fireplace to HA via the Bond Home integration (local control, no cloud required after setup)
- Bond can handle on/off; fan speed via RF remotes with speed buttons
- HA integration: `switch.fireplace`, `fan.fireplace_blower` (if remote supports speed)

**Pros:** Clean, no wiring into gas valve circuit, works through fireplace's own safety systems
**Cons:** Requires an RF remote kit if one doesn't exist (~$40–80); Bond Bridge is another hub

### Option B — Smart Relay for ignition + Smart Fan Controller for blower (most capable)

**For the on/off (millivolt switch):**
- **Shelly 1** (~$15, ESPHome-flashable) or **Aeotec Nano Switch** (Z-Wave, ~$40) wired as a dry-contact relay across the existing wall switch
- The relay doesn't carry load — it just closes the circuit the same way the switch does
- Fully local via ESPHome or Z-Wave; no cloud dependency
- ⚠ **Safety note:** Wire in parallel with the existing switch (not replacing it) so manual control is always available

**For the blower (click-on + variable dial):**
The dial is a standard fireplace blower speed controller with integrated on/off — a single unit that clicks on then varies speed. Two automation strategies:

**Strategy 1 — Replace the dial with a smart fan speed controller (full variable control):**
- Remove existing dial; install a Zigbee/Z-Wave fan speed controller in its place
- Standard dimmers CANNOT be used — they damage PSC motors
- Inovelli VZW36 is designed for PSC fan motors (ceiling fans) — may work if motor wattage is within spec (~25–65W typical for fireplace blowers) and wiring is accessible at the dial location
- Gives HA full speed control (low/medium/high or %)

**Strategy 2 — Smart relay in series, dial left at fixed speed (simpler):**
- Turn the existing dial to desired speed and leave it there
- Add a smart relay (Shelly 1, Aeotec Nano, or Zigbee equivalent) in series with the dial's power feed
- HA controls power on/off; the dial determines the fixed speed
- Loses variable speed but gains reliable on/off automation with minimal wiring
- Practical if you always run the blower at one speed anyway

**Pros:** Full variable speed control in HA, ZHA/Z-Wave native, no new hub
**Cons:** Requires wiring work; millivolt relay needs careful installation

### Option C — Smart Plug for blower only (simplest partial solution)

If the blower has a separate 120V power cord (some do):
- Smart plug controls blower power on/off (no variable speed — fixed at whatever the dial is set to)
- On/off switch still handled manually or via Option A/B
- Very low effort, very limited capability

---

## Recommended Path

**Determine switch type first.** If it's millivolt (most likely for a 2005–2006 gas fireplace):

1. Add a **Skytech RF remote kit** + **Bond Bridge** for the on/off — this is the safest path for the gas valve
2. Replace the blower dial with a **smart fan speed controller** (after confirming motor specs) — or leave the dial at a fixed position and just toggle power

If the on/off is 120V (less common), a Shelly or Z-Wave relay is simpler and cheaper.

---

## HA Integration Target

```yaml
# Entities once integrated
switch.fireplace          # gas on/off
fan.fireplace_blower      # speed control (low/medium/high or 0-100%)
```

**Automation ideas:**
- Turn off automatically if no motion detected in family room for 30 min
- Turn on via voice ("Hey Alexa, start the fireplace")
- Turn off when alarm armed away
- Dashboard button card: flame icon, toggle + speed slider

---

## Safety Notes

- Never automate the gas valve in a way that bypasses the fireplace's own ignition safety systems
- Always maintain a manual override — keep the physical switch functional in parallel
- Do not use the fireplace relay to "restart" the fireplace if it fails to ignite; that should remain a manual action
- Ensure any smart device installed near the firebox is rated for the ambient temperature in that location

---

## Open Questions

- [ ] What is the fireplace make/model?
- [ ] Is the on/off switch millivolt or 120V?
- [ ] Does an RF remote receiver already exist inside the unit?
- [ ] What is the blower motor wattage?
- [ ] Is the blower dial on the same wall plate as the on/off switch?

*Last updated: 2026-05-21*
*Status: **Deferred — revisit in fall/winter.** Switch type confirmed (millivolt). Blower motor wattage still needed before ordering hardware.*
