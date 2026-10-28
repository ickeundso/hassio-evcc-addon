# Changelog

## 0.318.0-use-ml.35

- Fix: a heat pump charging hot water on its own was boosted although
  there was no surplus. In solar and smart mode evcc counted the heat
  pump's own consumption as surplus it could hand out, so every natural
  hot water charge could be extended by a boost, for example from the
  home battery. A heat pump that runs on its own no longer counts as
  surplus. Real solar surplus starts the boost as before.

## 0.318.0-use-ml.34

- Fix: while the optimizer controls the home battery, a heat pump no
  longer starts or keeps running on the home battery because of the
  battery settings "start charging from 60 %" and "continue down to
  35 %". The optimizer plans the battery, so such a boost worked against
  its plan, for example at a high price in the evening. Solar surplus,
  cheap grid prices and the manual battery boost work as before. Vehicles
  and setups without the optimizer are unchanged.
- Heat pumps with Viessmann readings: the hot water run recording keeps a
  run together across a defrost and records every boost as a whole, from
  switching on to off, with its energy, whether the heat pump had finished
  and the hot water temperature at the start. evcc learns from these
  records how much energy a run and a boost take and at which hours the
  heat pump charges the hot water on its own. Nothing is switched by this
  yet, the debug log shows what was learned.
- Boost planning for heat pumps, shadow mode only: after every optimizer
  run evcc decides whether a boost would be worth it now, later or not at
  all, at the real cost of the electricity in the optimizer plan, including
  energy from the home battery. It only boosts in smart mode, with at least
  5 ct expected saving and 12 hours between two boosts. Nothing is
  switched, the decisions are recorded and logged as "boost planning
  (shadow)".

## 0.318.0-use-ml.33

- Fix: the hot water run recording of 0.318.0-use-ml.32 saw no runs on
  heat pumps without the charging flag, among them the Vitocal 250-A.
  evcc now detects the hot water charge by the position of the heat
  pump's switching valve.

## 0.318.0-use-ml.32

- Fix: with SG Ready relays, the hot water temperature above the heat
  pump's limit kept every boost off, at cheap prices and with pv surplus.
  In the relay variant the heat pump limits itself through its SG 3
  setpoints, so evcc applies no temperature limit there and no longer
  writes the hot water target to the Viessmann. The loadpoint shows the
  heat pump's own target instead of a limit selection. The API variant
  is unchanged.
- New add-on option "Log level" sets the level evcc writes to the add-on
  log. Under Logs in evcc, the download button now offers the complete
  add-on log in addition to the recent entries kept in memory. The add-on
  needs access to the Supervisor API for this.
- SG Ready of the Viessmann device can be switched off in the advanced
  settings. The relays stay configured and are set back to normal once,
  after that evcc only shows the readings and never switches the heat
  pump. The loadpoint then hides the mode selection.
- Heat pumps with Viessmann readings: evcc records every hot water run,
  whether it started on its own or during a boost, with duration, energy
  and how quickly the heat pump reacted to the boost. It also records how
  old the Viessmann data is. Nothing is switched by this, the data
  prepares the boost planning.

## 0.318.0-use-ml.31

- Viessmann heat pumps can be controlled through SG Ready with two Shelly
  relays (gen 2 or newer) on the contacts 143.5 and 143.4. The new
  advanced settings "SG Ready boost relay" and "SG Ready dim relay" of the
  Viessmann device replace the one-time hot water charge of the API: boost
  closes 143.5 only (SG 3, raised setpoints for heating, buffer and hot
  water), dim closes 143.4 only (SG 1, used for § 14a only). Power and
  temperatures are still read from the Viessmann API. Without the new
  settings nothing changes.

## 0.318.0-use-ml.30

- Update to evcc 0.318.0 plus the fixes made upstream since then, among
  them: the home battery is held when its energy is discharged later at a
  higher price, and the remaining charge for a plan goal is estimated while
  the plan slot is active.
- Fix: with the optimizer in automatic mode, a full home battery was
  switched to normal mode while the car was fast charging and discharged
  with full power into the car, because the plan contained a token
  discharge of a few watts. A planned discharge far below what normal mode
  would deliver now keeps the battery on hold.
- Fix: the optimizer planned every vehicle with voltage × minimum current ×
  phases as its minimum power. A Renault ZOE at 8 A draws 3.5 kW instead of
  the nominal 5.5 kW, so the plan carried too much energy per quarter hour
  and, with demand peak attenuation on, the car never fit into the current
  quarter hour: it was started at the slot change and stopped a few minutes
  later, every quarter hour. evcc now measures the charge power at minimum
  current while charging, stores it with the vehicle per phase count, and
  plans with it.

## 0.317.1-use-ml.29

- Fix: if the sponsor check failed at startup because the sponsor server
  could not be reached, evcc stayed on "sponsorship unavailable" until the
  next restart, and every optimizer run failed with "Missing authorization
  header". evcc now checks the token again after 1, 2, 5 and 15 minutes and
  then every hour, and shows the sponsor again as soon as the check works.

## 0.317.1-use-ml.28

- Fix: restoring the solar forecast settings at startup no longer starts an
  optimizer run before the meters are read. That run always failed and left
  one error entry per restart in the optimizer comparison.
- Changed: the log now shows each step of a home battery decision: what the
  optimizer suggests, what the control cycle applies and, after a charge
  command, whether the inverter has grid charging enabled (Huawei).

## 0.317.1-use-ml.27

- Fix: as long as a roof plane has no shading mask of its own yet, its
  forecast uses the mask learned from the total. Until now the whole
  forecast fell back to the daily scale in that case.

## 0.317.1-use-ml.26

- New: the solar forecast learns where the house and trees shade the
  modules. It compares measured and forecast power for each sun position
  and corrects every forecast quarter hour accordingly, instead of using a
  single daily factor. Positions the sun has not reached yet use the nearest
  known one, so the correction also works ahead into winter.
- New: with one meter per inverter string (Huawei SUN2000, MPPT input pv1 or
  pv2) and one solar tariff per roof plane, each plane gets its own shading
  mask. The meters are assigned to the tariffs via `POST /api/solarplanes`.

## 0.317.1-use-ml.25

- New: evcc records more heat pump values from Viessmann: flow, current heat
  output and power consumption, compressor speed setpoint, inlet pressure,
  heating curve, SCOP and the daily heat and electricity counters. The hot
  water tank temperature is shown in the heating water chart, the other
  values are available in the history for the optimizer.

## 0.317.1-use-ml.24

- Changed: the evcc page no longer renews an expired Home Assistant session
  by itself. The fix for reconnecting after Home Assistant was in the
  background stays. Renewing the session is up to the Home Assistant
  frontend.

## 0.317.1-use-ml.23

- Changed: the price forecast chart on the Energy page now is a card like
  the other sections, with the legend below the chart. It only appears once
  there is a forecast to show for the selected period.

## 0.317.1-use-ml.22

- Update to evcc 0.317.1 plus the fixes made upstream since then.
- Changed: the optimizer's automatic mode now has three levels: off, home
  battery only, or home battery and charging points. An existing "on" becomes
  "home battery and charging points", so nothing changes until you choose.
- Changed: the optimizer runs every 5 minutes in automatic mode.
- Fix: the home battery is no longer blocked from charging when the plan
  shows a tiny grid export that is only rounding noise, and not when it has
  reached its upper limit anyway.
- Changed: the History page is replaced by the new Energy page with flows,
  costs and consumers. The price forecast chart moved there.
- Changed: the database uses write-ahead logging. Two extra files next to
  the database (-wal, -shm) are normal.

## 0.316.1-use-ml.21

- New: the history view shows a price forecast chart. For each quarter hour
  it compares the actual grid price with the value the forecast gave two and
  three days earlier. The month and year views show the mean deviation per
  day, so a forecast that gets worse over time is easy to spot. Data is
  collected from now on and needs a grid tariff that reaches further than
  two days, for example a merged tariff with a price forecast.
- Fix: a merged tariff no longer fails completely when one of its two
  sources is down at startup. The other source carries the tariff and the
  missing one is retried every 15 minutes.

## 0.316.1-use-ml.20

- Fix: after Home Assistant was in the background for a longer time, the
  evcc page stayed on "not connected" until it was opened again. The page
  now renews the expired Home Assistant session itself and reconnects.

## 0.316.1-use-ml.19

- Fix: single wrong meter readings, far too low or far too high, are no
  longer recorded as energy. Until now one such reading could book the whole
  lifetime counter or millions of kWh into one quarter hour. This happened
  with the pv, grid and battery meters and once stopped the optimizer.
- Fix: the evcc page no longer flickers between "no connection" and the
  normal view after Home Assistant was in the background for a longer time.
  It now waits calmly until the connection really works again.

## 0.316.1-use-ml.18

- New: hovering a bar in the timeline charts shows since when and until when
  the state was on and for how long. The compressor adds the heat pump's
  consumption in that time, the heating rod its own, a room's heat request
  the lowest temperature. The legend shows the total time in the range.
- New: the optimizer dialog shows the consumption assumption and lets you
  change it. A higher value makes the optimizer expect more consumption and
  keep a reserve in the home battery.

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