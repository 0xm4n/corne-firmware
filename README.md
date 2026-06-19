# Corne ZMK Firmware

[![Build firmware](https://github.com/0xm4n/corne-firmware/actions/workflows/build.yml/badge.svg)](https://github.com/0xm4n/corne-firmware/actions/workflows/build.yml)

ZMK firmware configuration for a wireless Corne split keyboard using nice!nano
v2 controllers. The keyboard advertises itself as `Corne View`.

## Hardware Targets

The GitHub Actions workflow builds these targets:

| Board | Shield | Purpose |
| --- | --- | --- |
| `nice_nano//zmk` | `corne_left` | Left (central) half |
| `nice_nano//zmk` | `corne_right` | Right (peripheral) half |
| `nice_nano//zmk` | `settings_reset` | Clear saved ZMK settings |

The left half is the central half. Connect or pair the keyboard through the left
half during normal use.

## Features

- Three keymap layers: default, lower, and raise.
- Five selectable Bluetooth profiles and a profile-clear key.
- OLED widgets for output, layer, typing speed, and battery status.
- Sleep after 15 minutes of inactivity.
- Increased Bluetooth controller transmit power.
- Optional RGB underglow configuration, disabled by default.

See the [user guide](docs/user-guide-and-troubleshooting.md#default-keymap) for
the complete keymap tables and layer controls.

## Repository Layout

| Path | Description |
| --- | --- |
| [`config/corne.keymap`](config/corne.keymap) | Key bindings and layers |
| [`config/corne.conf`](config/corne.conf) | ZMK feature configuration |
| [`config/west.yml`](config/west.yml) | ZMK west manifest |
| [`build.yaml`](build.yaml) | Firmware build matrix |
| [`.github/workflows/build.yml`](.github/workflows/build.yml) | GitHub Actions workflow |

## Build

Firmware builds automatically on every push and pull request. You can also run
the workflow manually from the repository's **Actions** tab.

1. Open **Actions** and select the latest successful workflow run.
2. Download the `firmware` artifact.
3. Unzip it to get the left, right, and settings-reset `.uf2` files.

This configuration tracks ZMK's `main` branch. Board identifiers or other build
requirements can change when upstream ZMK introduces breaking changes.

## Flash

Flash each half separately:

1. Connect the half with a USB data cable.
2. Double-press its reset button to mount the `NICENANO` drive.
3. Copy the matching left or right `.uf2` file to that drive.
4. Wait for the controller to flash and restart.

For complete instructions, including resetting Bluetooth and split settings,
read [How to Flash Firmware](docs/how-to-flash-firmware.md).

## Customize

Edit [`config/corne.keymap`](config/corne.keymap), then commit and push the
change to trigger a new build. Every layer must contain exactly 42 bindings.

The [keymap editing guide](docs/how-to-edit-keymap.md) explains the layer
structure, ZMK behaviors, Bluetooth controls, and common changes.

## Documentation

- [Edit the keymap](docs/how-to-edit-keymap.md)
- [Build and flash firmware](docs/how-to-flash-firmware.md)
- [User guide and troubleshooting](docs/user-guide-and-troubleshooting.md)
