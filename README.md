# evcc Feature Branch - Home Assistant App

This repository contains a feature-branch build of [evcc](https://github.com/evcc-io/evcc) as a Home Assistant App.

## Fix: Min+PV respects bufferSoc

In the official version, Min+PV mode always charges at minimum current regardless of house battery
state. When there is no solar and grid prices are too high, this drains the house battery to power
the vehicle charger.

This fix makes Min+PV respect the `bufferSoc` threshold: when `bufferSoc` is configured and the
house battery drops below it, Min+PV stops charging instead of draining the battery.

See the [evcc/](evcc/) directory for app details and changelog.
