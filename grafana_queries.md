# Grafana Flux Queries — Home Analytics

These are copy-paste queries for Grafana panels.
Data source: InfluxDB 2 (Flux query language)
Bucket: homeassistant

---

## Temperature by Zone (Line Chart)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "sensor.bedroom_temperature" or
      r.entity_id == "sensor.upstairs_temperature" or
      r.entity_id == "sensor.dad_s_new_echo_temperature"
  )
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> yield(name: "temperature")
```

---

## Air Quality — CO₂, VOC, AQI (Line Chart)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "sensor.bedroom_carbon_dioxide" or
      r.entity_id == "sensor.bedroom_volatile_organic_compounds" or
      r.entity_id == "sensor.bedroom_air_quality_index"
  )
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> yield(name: "air_quality")
```

---

## Humidity (Line Chart)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) => r.entity_id == "sensor.bedroom_humidity")
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> yield(name: "humidity")
```

---

## Energy — Live Power Draw per Plug (Line Chart)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "sensor.front_room_plug_1_power" or
      r.entity_id == "sensor.upstairs_hallway_1_power" or
      r.entity_id == "sensor.upstairs_hallway_plug_2_power" or
      r.entity_id == "sensor.office_plug_1_power"
  )
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> yield(name: "power")
```

---

## Energy — Total kWh Consumed (Stat panels, one per plug)

```flux
// Front Room
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) => r.entity_id == "sensor.front_room_plug_1_summation_delivered")
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> last()
```

(Repeat for upstairs_hallway_1, upstairs_hallway_plug_2, office_plug_1)

---

## Garage Door Events (State Timeline)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "cover.left_garage_door_opener_garage_door" or
      r.entity_id == "cover.right_garage_door_opener_garage_door"
  )
  |> filter(fn: (r) => r._field == "value")
  |> yield(name: "garage")
```

---

## Presence — Who's Home (State Timeline)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "person.louis" or
      r.entity_id == "person.lindsay"
  )
  |> filter(fn: (r) => r._field == "value")
  |> yield(name: "presence")
```

---

## Leak Sensor History (State Timeline)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "binary_sensor.kitchen_sink_leak_detector" or
      r.entity_id == "binary_sensor.laundry_room_leak_sensor" or
      r.entity_id == "binary_sensor.master_bathroom_left_sink_leak_detector" or
      r.entity_id == "binary_sensor.master_bathroom_right_sink_leak_detector" or
      r.entity_id == "binary_sensor.powder_room_leak_sensor" or
      r.entity_id == "binary_sensor.office_sink_leak_detector" or
      r.entity_id == "binary_sensor.bedroom_3_sink_leak_detector"
  )
  |> filter(fn: (r) => r._field == "value")
  |> yield(name: "leaks")
```

---

## Phone Battery Levels (Line Chart)

```flux
from(bucket: "homeassistant")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "state")
  |> filter(fn: (r) =>
      r.entity_id == "sensor.louis_iphone_battery_level" or
      r.entity_id == "sensor.lindsays_iphone_battery_level"
  )
  |> filter(fn: (r) => r._field == "value")
  |> toFloat()
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> yield(name: "battery")
```
