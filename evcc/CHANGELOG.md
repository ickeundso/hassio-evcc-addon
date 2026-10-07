# Changelog

## 0.316.1-use-ml.18

- New: hovering a bar in the timeline charts shows since when and until when
  the state was on and for how long. The compressor adds the heat pump's
  consumption in that time, the heating rod its own, a room's heat request
  the lowest temperature. The legend shows the total time in the range.

## 0.316.1-use-ml.17

- Changed: Viessmann charts use one central set of colors. The colors are
  clearly apart now, rooms no longer share a color, and a series keeps its
  color in every chart.
- New: clicking legend entries highlights them in every chart, several at a
  time.
- Changed: range filter with 2 h, 4 h, 12 h and 24 h, calendar ranges and a
  free span including the time.
- Changed: pump, fan and compressor speed share one chart, the speed on a
  left scale.
- Fix: heat demand of a room holds until it is 0.3 K above its setpoint, as
  the ViCare app shows it.
- New: the stored request and responses of an optimizer run can be read via
  the API (`/api/ab/run/{id}`, with login). This allows replaying runs the
  solver ended without a proven optimum.

## 0.316.1-use-ml.16

- Changed: latest upstream fixes merged, among them: battery-supported
  charging ends once the battery can no longer avoid grid import, the always
  charge dropdown no longer switches to smart just by opening it, and Solcast
  saves quota at night.

## 0.316.1-use-ml.15

- New: heat demand timeline on the Viessmann page. It shows when the heating
  circuit requests heat (target supply above 0 °C) and which rooms are below
  their setpoint. The heating water panel is shaded while the circuit requests
  heat, the target supply is in the tooltip.
- New: room setpoints and the circuit target supply are recorded. Setpoints
  recorded so far are taken over once after the update.
- Fix: the heating circuit hint no longer names circuit 1.

## 0.316.1-use-ml.14

- Changed: based on evcc 0.316.1 plus the latest upstream fixes, among them
  the optimizer battery limit without soc limits, tariff cache before the
  first fetch, battery boost with several loadpoints and the confirmation
  before battery grid export.
- Fix: heating circuits are numbered from 1 as in Viessmann. The API counts
  from 0, so the mixed circuit shows as heating circuit 2.

## 0.316.0-use-ml.13

- Changed: energy panel no longer stacks. Heat pump and house each start at
  zero, so the house base load shows as a flat line; the total without wallbox
  is a dashed line, and the title total no longer counts it twice.
- New: second state chart for exclusive modes: heating program and 4/3-way
  valve position (one row each) plus hot water one-time charge. Colors are
  unique within each chart.
- Fix: the daily refresh of all Viessmann timestamps (around 21:12) no longer
  shows up as short on/off marks. Condensate pan and fan ring heaters, which
  report active all day, are left out.
- Changed: consistent labels: heat pump supply/return (heat pump circuit),
  buffer tank, supply heating circuit n, outdoor temperature (heat pump); the
  heating circuit hint no longer assumes underfloor heating only.

## 0.316.0-use-ml.12

- New: more operating lanes: evaporator, crankcase, condensate pan and fan
  ring heaters, hot water heating and one-time charge, active heating program
  (shows the night setback) and 4/3-way valve position. The heating rod has its
  own lane right below defrost. Heater flags get exact change times like
  defrost.
- New: the outdoor fan (%) joins the secondary pump panel ("Pump & fan").
- New: open windows (contact or detected) are shaded in the room's color in the
  room temperature panel.
- Changed: defrost is blue, hot water aqua.
- Fix: the operating state panel resizes with its lane count.

## 0.316.0-use-ml.11

- New: the room temperature panel shows the heat pump's own outdoor
  temperature as a dashed reference line, recorded from now on with the heating
  water temperatures (no extra Viessmann call).

## 0.316.0-use-ml.10

- New: defrost lane in the operating state timeline. Besides the 15 min
  snapshot of heating.outdoor.defrosting, the exact start and end times are
  taken from the flag's change timestamp, so a defrost of a few minutes shows
  even between two polls (an end without a seen start as a short tick).

## 0.316.0-use-ml.9

- New: the heat pump power includes the share Viessmann does not report:
  standby (default 25 W), heating circuit pump while running (default 7 W) and
  the secondary pump by its speed (default max. 60 W). All three are advanced
  charger settings; set them to 0 to use Viessmann's value unchanged.
- New: operating state timeline (heating, hot water, standby, defrost,
  compressor on, heating circuit pump on) plus secondary pump and compressor
  speed panels, recorded as 15 min snapshots with the rooms.
- Changed: the energy panel names its stacked series unambiguously: house
  (without heat pump), total without wallbox.

## 0.316.0-use-ml.8

- Changed: the optimizer forecasts the Viessmann heat pump from a 7-day profile
  scaled by the outdoor temperature forecast (demandtemperature) instead of the
  28-day average, so cold nights are planned with more heat pump demand and the
  home battery is kept for them.

## 0.316.0-use-ml.7

- New: time ranges 24 h, 1 week, current month, last month and a custom date
  span; an icon button reloads the charts from the stored data.
- New: the energy panel shows the consumption over the selected range per
  series in the legend and the total in its title.
- New: heating rod periods are shaded in the energy panel, derived from its
  daily consumption counters (recorded with the heating water temperatures, no
  extra Viessmann call). Shown separately, not part of the total.
- Changed: the filter bar of the API list sits below the charts.

## 0.316.0-use-ml.6

- New: the Viessmann API page charts the recorded history in three panels with
  one shared time axis: room temperatures (one color per room), heating water
  (supply, return, buffer tank, circuit supply) and energy per 15 minutes (house
  without loadpoints, heat pump). Hovering shows the same moment in all panels.
  Time range 24 h, 7 or 30 days; a table lists latest, min and max per room.
- New: supply, return, buffer and circuit supply temperatures are recorded with
  the rooms, once per 15 minutes (one more Viessmann call per slot).

## 0.316.0-use-ml.5

- Fix: the value column on the Viessmann API page no longer collapses to a few
  characters. Device messages show one field per line, timestamps are shortened.

## 0.316.0-use-ml.4

- New: settings → system links to the Viessmann API page, so it is reachable from
  the Home Assistant panel. Shown only when a Viessmann charger is configured.
- New: single-room temperature logging from the Viessmann room control
  (temperature, humidity, setpoint, window state), once per 15 minutes. Enable it
  and name the rooms on the Viessmann API page; history is available as CSV.
- Changed: values on the Viessmann API page (series, schedules, messages) are
  shown as readable lines instead of raw JSON.

## 0.316.0-use-ml.3

- New: page `/#/viessmann` listing everything the Viessmann API reports for the
  heat pump — readable values and writable commands with their parameters and
  limits. Read only, no command is executed. Requires login.

## 0.316.0-use-ml.2

- Changed: Viessmann heat pump is polled every 3 minutes instead of every minute,
  saving two thirds of the daily API quota shared with the ViCare app. Power and
  hot water readings can be up to 3 minutes old.

## 0.316.0-use-ml.1

- Changed: merged evcc 0.316.0 (includes 0.315.1 and 0.315.2).
- The upstream heating demand profile (#28232) replaces the branch's earlier
  version; the household base load still prefers the same-weekday profile.
- Fix: the add-on now reports its real version instead of a git-derived dev string.

## 0.315.0-use-ml.2

- New: `GET /api/ab/entities` lists the metric entities with their id, group,
  title and `isTemp` flag — needed to tell which loadpoint is the heat pump.

## 0.315.0-use-ml.1

- New: authenticated CSV export at `GET /api/ab/export?from=…&to=…` — pulls the
  A/B shadow data off the add-on without copying the whole evcc.db. Needs an API
  key in the `Authorization: Bearer …` header; `from`/`to` are mandatory.
- Changed: merged evcc 0.315.0. This includes the upstream Mode Redesign —
  `pv` and `minpv` are deprecated input aliases now and normalize to `smart` plus
  always charge. The bufferSoc drain protection follows always charge and behaves
  as before.
- Changed: the add-on is aarch64 only; the amd64 image is no longer published.

_Note: releases between 0.303.2-use-ml.4 and this one were not tracked here._

## 0.303.2-use-ml.4

- Fix: TLS verification disabled for A/B optimizer client — works with self-signed LAN certs
- Both http:// and https:// now work for ML_OPTIMIZER_URI

## 0.303.2-use-ml.3

- Fix: A/B optimizer logs now appear at INFO level (were invisible at default log config)
- New: startup banner confirms ML_OPTIMIZER_URI is active and shows configured backends
- For real A/B comparison, set BOTH OPTIMIZER_URI and ML_OPTIMIZER_URI in the config tab

## 0.303.2-use-ml.2

- New: A/B optimizer shadow evaluation — when ML_OPTIMIZER_URI is set, evcc calls both MILP and ML optimizer backends in parallel every ~2 minutes and persists the results to SQLite for empirical comparison
- New: ML_OPTIMIZER_URI config option in the add-on configuration tab
- Includes all fixes from 0.303.2-feature (bufferSoc drain fix)

## 0.303.2-use-ml.1

- New: A/B optimizer harness infrastructure (SQLite tables, parallel backend orchestrator)
- Fix: Min+PV mode now respects bufferSoc — stops draining house battery when SOC drops below threshold

## 0.303.2-feature

- Fix: Min+PV mode now respects bufferSoc — stops draining house battery when SOC drops below threshold
- When bufferSoc is configured and battery is below it, Min+PV falls through to PV surplus logic instead of unconditionally charging at minimum current
- No change in behavior when bufferSoc is not configured (original Min+PV behavior preserved)