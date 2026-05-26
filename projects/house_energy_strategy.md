# House Energy Intelligence Strategy
## Claude Code Workstream Guide

---

## House Context

| Property | Value |
|---|---|
| Size | 4,800 sq ft |
| Floors | 3 |
| Year built | 2006 |
| Occupancy | New to owner — baseline behavior unknown |
| Panels | Two independent 200A services |
| Monitoring | 2x Refoss EM16P (one per panel) |
| Thermostat | Ecobee (TOU-aware) |
| Smart home | Home Assistant OS (HAOS) |
| Energy rates | Time-of-use plan — confirm exact windows and rates with utility |
| Gas | Yes — furnaces on floors 1 and 2 are gas. Gas costs are outside this strategy's scope. |
| Solar | None currently |
| EV | None currently |

---

## HVAC Inventory

| System | Type | Fuel | Panel | Monitored circuits |
|---|---|---|---|---|
| Floor 1 | Gas furnace + AC condenser | Gas + Electric | Panel 1 | Condenser only (CT 9+10) |
| Floor 2 | Gas furnace + AC condenser | Gas + Electric | Panel 1 | Condenser only (CT 11+12) |
| Floor 3 | Heat pump — air handler + condenser | Electric only | Panel 1 | Air handler (CT 1+2) + Condenser (CT 5+6 or confirm) |

> **Note:** Gas furnace blower motors draw minimal electricity. EM16P will not reliably detect furnace run state via wattage. Furnace runtime detection requires either a separate CT on the blower circuit or a thermostat-based approach via Ecobee integration.

---

## CT Sensor Mapping

### Panel 1 — Main appliances (200A)

| CT slot | Circuit | Type | Breaker |
|---|---|---|---|
| Main A | Service leg 1 | 200A main | — |
| Main B | Service leg 2 | 200A main | — |
| 1 | 3rd floor air handler — leg A | 240V double-pole | 50A |
| 2 | 3rd floor air handler — leg B | 240V double-pole | 50A |
| 3 | Stove — leg A | 240V double-pole | 40A |
| 4 | Stove — leg B | 240V double-pole | 40A |
| 5 | AC condenser 3 — leg A | 240V double-pole | 35A |
| 6 | AC condenser 3 — leg B | 240V double-pole | 35A |
| 7 | Dryer — leg A | 240V double-pole | 30A |
| 8 | Dryer — leg B | 240V double-pole | 30A |
| 9 | AC condenser 1 — leg A | 240V double-pole | 30A |
| 10 | AC condenser 1 — leg B | 240V double-pole | 30A |
| 11 | AC condenser 2 — leg A | 240V double-pole | 30A |
| 12 | AC condenser 2 — leg B | 240V double-pole | 30A |
| 13 | Washer | 120V single-pole | 20A |
| 14 | Dishwasher | 120V single-pole | 20A |
| 15 | Microwave | 120V single-pole | 20A |
| 16 | Whirlpool | 120V single-pole | 20A |
| Harness | EM16P power | — | Bath GFI or Kit/Nook 20A standard breaker |

**Not monitored in Panel 1:** Kit/Nook receptacles, Kit/Dining receptacles, Bath GFI

### Panel 2 — Lighting and general (200A)

| CT slot | Circuit | Type | Breaker |
|---|---|---|---|
| Main A | Service leg 1 | 200A main | — |
| Main B | Service leg 2 | 200A main | — |
| 1 | Attic furnace | 120V single-pole | 15A |
| 2 | Crawl furnace | 120V single-pole | 15A |
| 3 | Fridge | 120V single-pole | 15A |
| 4 | Irrigation | 120V single-pole | 15A |
| 5 | Power vent | 120V single-pole | 15A |
| 6 | Family rm lts/receptacles | 120V single-pole | 15A |
| 7 | Living rm lts/receptacles | 120V single-pole | 15A |
| 8 | Master bath & lights | 120V single-pole | 15A AFCI |
| 9 | Master bed lts/receptacles | 120V single-pole | 15A AFCI |
| 10 | Bed #2-3 lts/receptacles | 120V single-pole | 15A AFCI |
| 11 | Up hall/bath & front BR #4-5 | 120V single-pole | 15A |
| 12 | Garage lts/receptacles | 120V single-pole | 15A |
| 13 | Outside lights | 120V single-pole | 15A |
| 14 | Kit/dining lights | 120V single-pole | 15A |
| 15 | Laundry nook lights | 120V single-pole | 15A |
| 16 | Attic/smokes | 120V single-pole | 15A |
| Harness | EM16P power | — | Existing spare 15A standard breaker (blank slot) |

**Not monitored in Panel 2:** Disposal, Foyer lights

---

## Entity Naming Convention

> **Action for Claude Code — Session 1:** Connect to HAOS, discover all Refoss EM16P entities, and populate this table with actual entity IDs before proceeding with any other workstream.

| Logical name | Expected entity ID | Panel |
|---|---|---|
| Whole-home power (combined) | `sensor.whole_home_power` (template) | Both |
| Panel 1 main power | TBD | Panel 1 |
| Panel 2 main power | TBD | Panel 2 |
| 3rd floor air handler | TBD | Panel 1 |
| Stove | TBD | Panel 1 |
| AC condenser 1 | TBD | Panel 1 |
| AC condenser 2 | TBD | Panel 1 |
| AC condenser 3 | TBD | Panel 1 |
| Dryer | TBD | Panel 1 |
| Washer | TBD | Panel 1 |
| Dishwasher | TBD | Panel 1 |
| Microwave | TBD | Panel 1 |
| Whirlpool | TBD | Panel 1 |
| Attic furnace | TBD | Panel 2 |
| Crawl furnace | TBD | Panel 2 |
| Fridge | TBD | Panel 2 |
| Irrigation | TBD | Panel 2 |

---

## TOU Rate Schedule

**Utility:** Dominion Energy Virginia — Schedule 1G (Residential TOU)

### Summer (May – September) — Weekdays
| Period | Hours | Rate ($/kWh) |
|---|---|---|
| Super Off-Peak | 12:00 AM – 5:00 AM | $0.124709 |
| Off-Peak | 5:00 AM – 3:00 PM and 6:00 PM – 12:00 AM | $0.142590 |
| **Peak** | **3:00 PM – 6:00 PM** | **$0.301638** |

### Winter (October – April) — Weekdays
| Period | Hours | Rate ($/kWh) |
|---|---|---|
| Super Off-Peak | 12:00 AM – 5:00 AM | $0.140752 |
| Off-Peak | 5:00 AM – 6:00 AM, 9:00 AM – 5:00 PM, 8:00 PM – 12:00 AM | $0.146795 |
| **Peak** | **6:00 AM – 9:00 AM and 5:00 PM – 8:00 PM** | **$0.262161** |

### Weekends & Holidays (Year-Round)
| Period | Hours | Rate ($/kWh) |
|---|---|---|
| Super Off-Peak | 12:00 AM – 5:00 AM | Summer: $0.124709 / Winter: $0.140752 |
| Off-Peak | 5:00 AM – 12:00 AM | Summer: $0.142590 / Winter: $0.146975 |
| Peak | None | — |

**Holidays:** New Year's Day, Memorial Day, Independence Day, Labor Day, Thanksgiving, Christmas

### HA Entities
| Entity | Purpose |
|---|---|
| `sensor.tou_period` | Current period: `peak`, `off_peak`, or `super_off_peak` |
| `sensor.tou_rate` | Current rate in $/kWh |
| `input_boolean.peak_mode` | `on` during peak hours — use this in automations |

---

## Guiding Principles for Claude Code

1. **Discovery before optimization.** Do not build TOU automations until 30 days of baseline data exists. Automating a house you don't understand yet creates false confidence.
2. **Measure before alerting.** Establish normal ranges for every circuit before building anomaly alerts. An alert without a baseline is noise.
3. **One panel, one session.** When making changes to automations or dashboards, scope each Claude Code session to one workstream to avoid conflicts.
4. **Never overwrite existing automations.** Before generating new automations, read existing ones. Append or replace specific automations by ID — never bulk-replace the automation file.
5. **Long-term statistics first.** Confirm long-term statistics recording is enabled for all EM16P sensors before any other work. Missing data cannot be recovered retroactively.
6. **Feed this file at session start.** Begin every Claude Code session by reading this file in full before connecting to HAOS or writing any code.

---

## Workstreams

---

### Workstream 0 — Foundation
**Do this before anything else. All other workstreams depend on it.**

#### Objective
Verify the EM16P installation is recording correctly, establish long-term statistics, create the whole-home combined sensor, and confirm entity names.

#### Tasks
- [ ] Connect to HAOS via MCP and discover all Refoss EM16P sensor entities
- [ ] Verify each branch CT is showing nonzero wattage on at least one active circuit
- [ ] Confirm long-term statistics recorder is enabled for all EM16P energy and power sensors
- [ ] Create `sensor.whole_home_power` template sensor summing both panel mains
- [ ] Create `sensor.whole_home_energy` template sensor for cumulative kWh
- [ ] Configure TOU rate schedule as a `schedule` helper with exact utility window times
- [ ] Create `input_boolean.peak_mode` toggled by the TOU schedule
- [ ] Populate entity naming table in this file with actual entity IDs
- [ ] Verify Ecobee integration is active and thermostat entities are reachable

#### Success criteria
- All 34 CT sensors (16 branch + 2 main per panel × 2 panels) returning live data
- Long-term statistics confirmed active — verify via Developer Tools > Statistics
- `sensor.whole_home_power` showing a plausible whole-home wattage
- `input_boolean.peak_mode` correctly flipping at utility rate window boundaries
- This document updated with all real entity IDs

#### Definition of done
All items checked. No CT sensor showing null or zero during a period when loads are active. Entity table fully populated.

#### Dependencies
- Both EM16P units physically installed and connected to WiFi
- Refoss RPC integration installed in HACS
- HAOS MCP server accessible to Claude Code

---

### Workstream 1 — Baseline Discovery
**Weeks 1-4. Observe only. No automations yet.**

#### Objective
Build a complete picture of how this house consumes electricity before attempting to optimize anything. Establish normal ranges for every monitored circuit.

#### Tasks
- [ ] Run nightly phantom load report: average draw per circuit between midnight and 5am
- [ ] Identify and document all phantom loads above 10W
- [ ] Calculate whole-home baseline floor (minimum draw when house is unoccupied/asleep)
- [ ] Log daily HVAC runtime per condenser in minutes
- [ ] Identify whirlpool inline heater behavior — does it draw power when not in use?
- [ ] Document peak vs. off-peak consumption split for first full billing cycle
- [ ] Identify any circuits with erratic or unexpected wattage patterns
- [ ] Create baseline dashboard (see Workstream 4)

#### Claude Code prompts to run

```
Prompt 1 — Entity verification:
"Read house_energy_strategy.md. Connect to my HAOS instance via MCP.
List all Refoss EM16P sensor entities. For each, show current wattage,
confirm long-term statistics is enabled, and flag any returning null or zero."

Prompt 2 — Phantom load audit:
"Pull wattage history for all Panel 1 and Panel 2 branch CT sensors
between midnight and 5am for the last 14 days. Calculate average draw
per circuit during those hours. Rank highest to lowest. Flag anything
above 10W as a phantom load candidate. Generate a markdown report."

Prompt 3 — Whirlpool audit:
"Pull wattage history for the whirlpool circuit for the last 14 days.
Identify any periods of nonzero draw. Flag if it shows draw during hours
when it is unlikely to be in use (e.g. overnight). This may indicate an
inline heater running continuously."

Prompt 4 — Peak/off-peak split:
"Using my TOU schedule helper and Panel 1 + Panel 2 main CT history,
calculate total kWh consumed during peak vs. off-peak hours for the last
30 days. Express as a percentage split and estimate cost at my TOU rates."
```

#### Success criteria
- Phantom load inventory complete with wattage documented for each
- Whole-home baseline floor established (kW)
- Whirlpool behavior characterized — continuous heater yes/no
- Peak/off-peak split calculated for first full month
- Normal wattage ranges documented for all three condensers

#### Definition of done
Baseline report generated and saved to HAOS. Normal ranges documented in this file under a new section: **Established Baselines**. No unexplained anomalies outstanding.

#### Milestones
- Day 7: Phantom load audit complete
- Day 14: Whirlpool and always-on load characterization complete
- Day 30: First full month peak/off-peak split calculated
- Day 30: Baseline dashboard live

#### Dependencies
- Workstream 0 complete
- At least 14 days of recorded data
- Long-term statistics confirmed active before day 1

---

### Workstream 2 — HVAC Characterization
**Weeks 3-8. Overlaps with Workstream 1.**

#### Objective
Understand how each of the three HVAC systems performs, identify any anomalies, and establish runtime baselines that will enable predictive maintenance alerting.

#### Tasks
- [ ] Build daily runtime tracker for each condenser (minutes per day)
- [ ] Build runtime vs. outdoor temperature correlation for each condenser
- [ ] Detect short cycling: condenser on/off cycles shorter than 8 minutes
- [ ] Detect simultaneous heating/cooling conflict on 3rd floor heat pump
- [ ] Compare condenser runtime normalized by floor square footage
- [ ] Characterize attic and crawl furnace blower draw (Panel 2 CTs 1+2)
- [ ] Identify heat pump crossover temperature (outdoor temp at which 3rd floor air handler wattage spikes — indicates aux heat engagement)
- [ ] Build HVAC health dashboard (see Workstream 4)

#### Claude Code prompts to run

```
Prompt 1 — Runtime tracker:
"Create three input_number helpers to track daily runtime in minutes
for AC condenser 1, condenser 2, and condenser 3. Create automations
that increment each helper when the condenser wattage exceeds 200W and
reset at midnight. Push to HAOS."

Prompt 2 — Short cycle detection:
"Create an automation for each AC condenser that triggers a persistent
notification if the condenser turns on and off (wattage above then below
200W) within an 8-minute window. Label each notification with the
condenser name and timestamp. Push to HAOS."

Prompt 3 — Runtime vs temperature correlation:
"Pull 30 days of daily runtime history for each condenser and daily high
temperature from my weather integration. Calculate the correlation
coefficient for each system. Flag any system whose runtime does not
correlate with temperature — this may indicate a stuck relay, thermostat
issue, or refrigerant problem."

Prompt 4 — Heat pump conflict detection:
"Create an automation that monitors the 3rd floor air handler wattage.
If it exceeds 1000W during months October through April (heating season)
AND either condenser 1 or condenser 2 simultaneously exceeds 500W,
send a notification: 'Possible heating/cooling conflict detected — 3rd
floor heating while floor 1 or 2 condenser is running.'"
```

#### Success criteria
- Daily runtime logging active for all three condensers
- Short cycle detection automation live and tested
- Runtime vs. temperature correlation calculated — no system flagged as anomalous
- Heat pump conflict detection automation live
- Furnace blower wattage characterized for attic and crawl systems

#### Definition of done
HVAC dashboard live showing all three systems. Runtime vs. temperature curves plotted for at least 30 days. No unresolved short cycling events. Heat pump crossover temperature identified and documented in this file.

#### Milestones
- Week 3: Runtime trackers and short cycle detection live
- Week 5: First runtime vs. temperature correlation run
- Week 6: Heat pump conflict detection live
- Week 8: Full HVAC characterization report generated

#### Dependencies
- Workstream 0 complete
- Weather integration active in HAOS (provides outdoor temperature)
- Ecobee integration active (provides thermostat mode context)
- At least 21 days of data for meaningful correlation

---

### Workstream 3 — TOU Optimization Automations
**Weeks 5-12. Do not start until Workstream 1 baselines are established.**

#### Objective
Implement automations that actively shift load away from peak hours based on what Workstream 1 and 2 revealed about this house's actual behavior.

#### Tasks
- [ ] AC pre-cooling automation (Ecobee setpoint adjustment before peak)
- [ ] Peak load budget alert (whole-home draw threshold notification during peak)
- [ ] Dryer/washer peak-hour notification
- [ ] Dishwasher peak-hour notification
- [ ] Irrigation off-peak scheduling
- [ ] Outside lights off-peak scheduling
- [ ] Peak mode scene (single trigger that fires all peak-hour load reduction actions)
- [ ] Off-peak mode scene (single trigger that enables pre-conditioning and schedulable loads)

#### Claude Code prompts to run

```
Prompt 1 — Pre-cooling automation:
"Create an automation that triggers 90 minutes before peak hours begin.
Conditions: outdoor temperature above 75°F, current Ecobee setpoint above
70°F. Action: set Ecobee to 68°F. Create a second automation that restores
the setpoint to normal when peak hours begin. Use input_boolean.peak_mode
as the trigger state. Push to HAOS."

Prompt 2 — Peak load budget:
"Create an automation that monitors sensor.whole_home_power during peak
hours (when input_boolean.peak_mode is on). If whole-home draw exceeds
[threshold — set after Workstream 1 establishes baseline] watts, send a
notification listing the top 3 circuits by current draw."

Prompt 3 — Appliance peak alerts:
"Create automations for the washer, dryer, and dishwasher circuits. If
any of these circuits exceeds 200W during peak hours, send a notification
identifying which appliance started during peak and the estimated cost
premium vs. running it off-peak at my TOU rates."

Prompt 4 — Schedulable load scenes:
"Create two scenes: peak_mode_active and off_peak_mode_active. Peak mode:
turn off irrigation if running, send notifications for active high-draw
appliances, log peak-mode entry to a logbook entity. Off-peak mode: send
notification that off-peak has started, list recommended loads to run now.
Push to HAOS."
```

#### Success criteria
- Pre-cooling automation fires correctly on at least 3 consecutive peak days without false positives
- Peak load budget alert fires when threshold is exceeded and is silent otherwise
- Appliance notifications fire within 60 seconds of a peak-hour start
- Irrigation confirmed scheduling to off-peak windows only
- Peak and off-peak scenes execute without errors

#### Definition of done
All automations active for 14 consecutive days with no false positives or missed triggers. TOU performance dashboard shows measurable shift in peak vs. off-peak split compared to Workstream 1 baseline.

#### Milestones
- Week 5: Pre-cooling automation live (requires Workstream 1 thermal mass data)
- Week 6: Peak load budget alert live
- Week 7: Appliance notifications live
- Week 8: Schedulable load scenes live
- Week 10: First 30-day post-automation TOU split comparison
- Week 12: Automations tuned based on 30 days of real performance data

#### Dependencies
- Workstream 0 and Workstream 1 complete
- Thermal mass characterization from Workstream 2 (informs pre-cooling window)
- TOU rate schedule confirmed and configured in HAOS
- Ecobee integration confirmed writable (HA can set setpoints)

---

### Workstream 4 — Dashboards
**Runs in parallel with all workstreams. Build each dashboard when its data is ready.**

#### Objective
Build a hierarchy of Lovelace dashboards that give immediate operational visibility and long-term trend analysis without requiring manual data queries.

#### Dashboard 1 — Overview (build in Week 1)

Purpose: daily glance. Should answer "how is the house doing right now" in under 5 seconds.

Required cards:
- Whole-home current draw (gauge, 0-10kW)
- Current TOU period (peak/off-peak indicator with time until next transition)
- Today's cost so far (both panels combined, in dollars)
- Yesterday's cost (comparison reference)
- Peak/off-peak split today (mini bar)
- Top 3 circuits by current draw (auto-ranked entity list)

```
Claude Code prompt:
"Generate Lovelace YAML for an overview dashboard with the cards listed
in Workstream 4 Dashboard 1. Use my actual entity IDs from house_energy_strategy.md.
Use the energy-date-selection card for cost tracking. Push to HAOS as a
new dashboard named 'Energy Overview'."
```

#### Dashboard 2 — HVAC Health (build in Week 3)

Purpose: weekly check. Should answer "are my three systems healthy."

Required cards:
- Current wattage per condenser (3 gauge cards)
- Today's runtime per condenser (3 stat cards in minutes)
- 7-day runtime trend per condenser (3 mini sparkline history graphs)
- 3rd floor air handler current draw and mode indicator
- Attic furnace and crawl furnace current draw
- Short cycle event log (logbook card filtered to short cycle notifications)

#### Dashboard 3 — Circuit Detail (build in Week 2)

Purpose: investigation tool. Not for daily use.

Required cards:
- All 32 branch CTs across both panels in a sortable table
- Columns: circuit name, current watts, today kWh, today cost, 24h trend
- Filter controls for panel 1 / panel 2 / all

#### Dashboard 4 — TOU Performance (build in Week 5)

Purpose: monthly review. Should answer "is our behavior improving."

Required cards:
- Monthly peak vs. off-peak split (current month vs. last month)
- Cost by circuit — top 10 ranked by monthly spend
- Pre-cooling performance: condenser runtime during peak hours this month vs. baseline
- Automation effectiveness log: how many times each TOU automation fired this month
- Projected monthly bill based on current month pace

#### Success criteria
- All four dashboards accessible and loading without errors
- No hardcoded entity IDs — all reference actual discovered entity names
- Dashboards render correctly on both desktop and mobile

#### Definition of done
All four dashboards live. Overview dashboard set as default for the Energy tab. Each dashboard reviewed after 7 days of live data and adjusted for any missing or misleading cards.

---

### Workstream 5 — Financial Reporting
**Month 2 onward. Requires 60+ days of long-term statistics.**

#### Objective
Generate monthly cost allocation reports that tell you exactly what this house costs to run and where the money goes.

#### Tasks
- [ ] Monthly cost per HVAC system
- [ ] Monthly cost per major appliance
- [ ] Cost per dryer cycle (average)
- [ ] Cost per whirlpool use (if used)
- [ ] Whole-home cost vs. prior month with variance explanation
- [ ] Estimated annual run rate by system

#### Claude Code prompts to run

```
Prompt 1 — Monthly cost report:
"Using long-term statistics for all EM16P energy sensors and my TOU rate
schedule, calculate total electricity cost for the last full calendar month
broken down by circuit. Separate peak-hour cost from off-peak cost for each
circuit. Generate a markdown report sorted by total monthly cost descending.
Save as monthly_energy_report_[YYYY-MM].md."

Prompt 2 — Appliance cost per use:
"Identify individual dryer cycles in the last 30 days by detecting when
the dryer circuit exceeds 3000W and then drops below 100W. Calculate the
kWh consumed per cycle and the cost at peak vs. off-peak rates. Report
average cost per cycle and how many cycles ran during peak vs. off-peak."

Prompt 3 — Annual projection:
"Based on 60 days of whole-home energy consumption history, project annual
electricity cost at current TOU rates. Break this down by seasonal estimate
if sufficient data exists. Compare projected cost to EPA Energy Star
benchmarks for a 4,800 sq ft home built in 2006 in climate zone [confirm
your zone]."
```

#### Success criteria
- Monthly report generated within first 5 days of each month
- Cost per dryer cycle calculated with at least 20 cycle samples
- Annual projection within a reasonable range of utility estimate
- Reports saved to HAOS config directory for historical reference

#### Definition of done
Three consecutive monthly reports generated. Year-over-year comparison possible (requires 13 months of data — this is a long-term milestone).

#### Dependencies
- Workstream 0 complete with long-term statistics active from day 1
- Minimum 60 days of data
- TOU rate schedule confirmed with exact dollar values

---

### Workstream 6 — Future Expansion (placeholder)
**Plan now, implement when hardware arrives.**

These workstreams are not active but are documented here so Claude Code has context when the time comes.

#### EV charger (when added)
- Add CT slot to Panel 1 or Panel 2 (will require bumping a lower-priority circuit)
- Preferred bump candidates: Laundry nook lights (Panel 2 CT 15) or Attic/smokes (Panel 2 CT 16)
- Automation: charge only during off-peak or super off-peak windows
- Automation: pause charging if whole-home draw exceeds peak budget threshold
- Integration target: ChargePoint, Wallbox, or Emporia smart EVSE depending on purchase

#### Solar (when added)
- Add solar production CT to each panel's EM16P main sensor configuration
- Enable net metering tracking in HA energy dashboard
- Automation: surplus detection — when generation exceeds consumption, trigger water heater or EV charging
- Integration: Solcast solar forecast for predictive automation

#### Heat pump water heater (when current water heater needs replacement)
- Current water heater is gas — no TOU optimization possible now
- When replaced with Rheem ProTerra or AO Smith Voltex, add CT and off-peak heating automation
- Federal tax credit eligibility: confirm at time of purchase

---

## Established Baselines
*Populated by Claude Code as Workstream 1 progresses. Empty until data is collected.*

| Metric | Value | Date established |
|---|---|---|
| Whole-home baseline floor (midnight–5am) | TBD | — |
| Panel 1 peak-hour average draw | TBD | — |
| Panel 2 peak-hour average draw | TBD | — |
| AC condenser 1 normal range (cooling) | TBD | — |
| AC condenser 2 normal range (cooling) | TBD | — |
| AC condenser 3 normal range (cooling) | TBD | — |
| 3rd floor air handler normal range (cooling) | TBD | — |
| 3rd floor air handler normal range (heating) | TBD | — |
| Fridge baseline draw | TBD | — |
| Whirlpool inline heater behavior | TBD | — |
| Dryer average cycle kWh | TBD | — |
| Peak vs. off-peak split (month 1) | TBD | — |
| Thermal mass — minutes before condenser restart after pre-cool | TBD | — |
| Heat pump crossover temperature (°F) | TBD | — |

---

## Known Issues and Quirks
*Populated as discovered. Empty at project start.*

| Circuit | Observation | Date | Status |
|---|---|---|---|
| — | — | — | — |

---

## Milestone Summary

| Milestone | Target | Workstream | Status |
|---|---|---|---|
| Both EM16P units installed and recording | Day 1 | WS0 | — |
| Long-term statistics confirmed active | Day 1 | WS0 | — |
| All entity IDs documented | Day 2 | WS0 | — |
| TOU schedule helper configured | Day 2 | WS0 | — |
| Overview dashboard live | Week 1 | WS4 | — |
| Circuit detail dashboard live | Week 2 | WS4 | — |
| Phantom load audit complete | Week 2 | WS1 | — |
| HVAC runtime trackers live | Week 3 | WS2 | — |
| HVAC health dashboard live | Week 3 | WS4 | — |
| Short cycle detection live | Week 3 | WS2 | — |
| Whirlpool characterization complete | Week 2 | WS1 | — |
| First month peak/off-peak split calculated | Week 4 | WS1 | — |
| Workstream 1 baselines table fully populated | Week 4 | WS1 | — |
| Runtime vs. temperature correlation run | Week 5 | WS2 | — |
| Pre-cooling automation live | Week 5 | WS3 | — |
| TOU performance dashboard live | Week 5 | WS4 | — |
| All TOU automations live | Week 8 | WS3 | — |
| Automations tuned after 30 days | Week 12 | WS3 | — |
| First monthly cost report | Month 2 | WS5 | — |
| Annual cost projection | Month 3 | WS5 | — |
| Three consecutive monthly reports | Month 4 | WS5 | — |

---

## Session Start Checklist for Claude Code

Run this at the start of every Claude Code session before writing any code or connecting to HAOS:

1. Read this file in full
2. Confirm which workstream this session targets
3. Check the milestone summary — what is the current status?
4. Connect to HAOS via MCP and verify the EM16P entities are live
5. Read existing automations relevant to this session's workstream before generating new ones
6. Do not modify dashboards or automations outside the current workstream's scope
7. After completing work, update the milestone summary and known issues table in this file

---

*Last updated: 2026-05-25*
*Status: EM16P install overdue (was scheduled 2026-05-22); WS0 cannot proceed until hardware is installed and online*
