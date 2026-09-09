# Changelog

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