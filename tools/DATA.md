# Timetable data format

All files are UTF-8 JSON with three day-type arrays — `weekday`, `saturday`,
`holiday` — plus a `_meta` object (sources, revision date of the timetable,
extraction notes). Times are `"HH:MM"` local. Japanese public holidays use
the `holiday` table (the app embeds the JP holiday calendar).

## Bus legs — `bus_<from>_to_<to>.json`

```json
{ "weekday": ["06:12", "06:24"], "saturday": [], "holiday": [], "_meta": {} }
```

Plain departure-time arrays. Multiple routes serving the same stop pair are
merged into one sorted list (the board shows departures, not routes).
Ride time between the stops is a profile constant, not per-row data.

## Train boards (stopping trains only) — e.g. `seibu_hoya_up.json`

```json
{ "weekday": [{ "dep": "07:24", "type": "各停", "dest": "池袋" }], "_meta": {} }
```

## Full boards (incl. passing trains) — e.g. `seibu_hoya_up_full.json`

```json
{ "weekday": [{ "dep": "07:26", "type": "急行", "dest": "池袋", "stops": false }], "_meta": {} }
```

`stops: false` rows render as thin "通過" strips. Official timetables do not
print passing times; estimate them from the nearest timed stops on both sides
and document the method in `_meta.passing_time_method`. Accuracy target is
±1 min — passing rows are used for ordering only.

`_meta.dest_classification` should state how `dest` values map to service
classes (e.g. which destinations are Yurakucho-line through services vs.
Fukutoshin-line through services), because the recommendation logic keys off
that classification.

## Down-direction arrivals

Files like `seibu_kotake_down.json` / `seibu_ikebukuro_down.json` record, per
departure from the origin station, the arrival time at the target station
(`arr_hoya`), since down-direction run times vary by train type and stopping
pattern. The app derives per-type run times from these.

## Adding a station

1. Pick the official timetable source for the operator (station timetable
   page or poster PDF) and one independent cross-check source.
2. Extract per day type; verify the stopping-train subset against the
   second source row by row.
3. Add `_meta` with sources, the timetable's revision date, and any
   estimation methods used.
4. Register the station in the app's station library (Settings picker) with
   its coordinates and the legs it participates in.
