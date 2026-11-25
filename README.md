# MobilityInformation

把沿途的时刻牌，搬进手机。

A departure-board PWA for Tokyo commuters. Instead of computing an itinerary
for you, it shows you the boards you would see along the way — the bus board at
your stop, the train board at your station — and draws one honest arrow from
each departure to the first connection it can physically catch. Recommendations
are marked separately, never smuggled into the arrows.

## What it does

- **Home page** — your next 3 buses on the left; the full departure board at
  your station on the right (including trains that pass without stopping,
  folded into thin strips). A thin arrow links each bus to the first train it
  can catch; a separate "推奨" frame marks the recommended train, computed with
  a direct-train-first rule: a transfer only wins if it arrives at least
  15 minutes earlier.
- **Station page** — both directions on one page, stacked. When you're at the
  station you may be going either way, so both boards are there; the likely
  direction for the time of day comes first.
- **Work page** — the mirror image for the way home. Trains that don't reach
  your home station say so on their row, with the transfer point and the
  resulting arrival time printed inline. No hidden transfers.
- GPS never hijacks the screen: entering a station's geofence only raises a
  slim hint bar. A manually chosen page locks for 30 minutes.
- Works fully offline on static timetables. An optional ODPT consumer key
  (Settings) enables a live layer where supported.

## Install (phone)

Open the Pages site in Chrome (Android) or Safari (iOS) and choose
"Add to Home screen". It runs full-screen like a native app.

Live site: https://powergx-yu.github.io/MobilityInformation/

## Station library

Stations and timetables live in `data/` as plain JSON — the app logic is
station-agnostic. Bundled today: Hoya (Seibu Ikebukuro line + local buses),
Ikebukuro, Kotake-Mukaihara, Ichigaya (Yurakucho line). The default sample
profile is Hoya ⇄ Ichigaya; change both ends in Settings and set your own
home/work anchors by GPS capture.

To add a station, produce its timetable files in the documented format —
see [tools/DATA.md](tools/DATA.md).

## Design notes

- Arrows mean one thing only: *physically catchable*. Preference logic
  (direct-first, 15-minute rule, cross-platform transfers, the
  get-off-one-stop-early trick) lives in the recommendation frame, so the
  board never lies to you.
- Passing trains are shown because a real station board shows them; their
  times are estimates (±1 min, derived from neighbouring stops) and are only
  used for ordering.
- Data sources: official operator timetables (Seibu Bus / Seibu Railway /
  Tokyo Metro), cross-checked across two independent listings per file.
  Each data file carries its own `_meta` with sources and revision dates.
