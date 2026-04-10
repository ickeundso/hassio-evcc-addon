# Changelog

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