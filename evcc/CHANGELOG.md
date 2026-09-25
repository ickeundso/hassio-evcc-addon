# Changelog

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