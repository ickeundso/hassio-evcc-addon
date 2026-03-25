# evcc Feature Branch - Home Assistant App

This is a feature-branch build of [evcc](https://github.com/evcc-io/evcc) for testing purposes.

## Fix: Min+PV respects bufferSoc

In the official version, Min+PV mode always charges at minimum current regardless of house battery
state. When there is no solar and grid prices are too high, this drains the house battery to power
the vehicle charger.

This fix makes Min+PV respect the `bufferSoc` threshold: when `bufferSoc` is configured and the
house battery drops below it, Min+PV stops charging instead of draining the battery. Charging
resumes when solar surplus is available or the battery recovers above the threshold. The charging
plan and smart cost settings continue to handle scheduled charging during cheap tariff windows.

No change in behavior when `bufferSoc` is not configured.

## Usage

- **Stop** the official evcc app before starting this one (they share the same ports)
- Both apps share the same config file and database — configure the same paths
- **Start** this app to test the feature branch
- **Stop** this app and restart the official one to go back

## Image Source

Docker image is hosted on GitHub Container Registry: `ghcr.io/ickeundso/evcc-feature-{arch}`
