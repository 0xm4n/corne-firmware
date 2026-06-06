# Corne Wireless ZMK User Guide and Troubleshooting

This guide covers daily use, Bluetooth behavior, firmware updates, and common
issues for this Corne wireless ZMK configuration.

The firmware in this repository is built for:

- `nice_nano_v2` with `corne_left`
- `nice_nano_v2` with `corne_right`
- `nice_nano_v2` with `settings_reset`

The configured keyboard name is:

```text
Corne View
```

## Quick Start

1. Turn on both halves.
2. Connect the left half to your computer over USB, or use it as the Bluetooth
   half when pairing.
3. For Bluetooth, hold the lower layer key and select a Bluetooth profile.
4. Pair with `Corne View` from your computer, phone, or tablet.
5. Start typing after both halves are powered on and connected to each other.

If anything behaves strangely, remove old `Corne View` Bluetooth entries from
your host device, flash `settings_reset` to both halves, flash the normal
firmware again, and pair from a clean Bluetooth profile.

## Daily Use

### Power

If your Corne build has battery power switches, turn on both halves before use.
For long-term storage, turn both halves off and charge the batteries before
putting the keyboard away.

This firmware enables ZMK sleep:

```dts
CONFIG_ZMK_SLEEP=y
CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=900000
```

The keyboard can sleep after about 15 minutes of inactivity to reduce battery
drain. Press a key to wake it.

### Charging

Use a USB-C cable that supports data and power. Charging-only cables may charge
the controller but will not work for firmware flashing or USB typing.

For keyboards with battery switches, keep the relevant half powered on while
charging unless your hardware documentation says otherwise.

### Split Keyboard Behavior

A wireless split ZMK keyboard has one central half and one peripheral half.
The central half connects to the computer over USB or Bluetooth. The peripheral
half sends key events to the central half.

For this configuration, use the left half as the central half. Connect USB to
the left half for normal wired typing and pair Bluetooth from the left half.

Flash both halves with the correct firmware:

- Left half: `corne_left` firmware
- Right half: `corne_right` firmware

The peripheral half is not intended to work as a complete standalone keyboard.
If you connect the peripheral half over USB, that connection is mainly useful
for charging or flashing.

## Connection Modes

### USB

Connect the left half to your computer with a USB-C data cable. If the
firmware is running normally, the keyboard should type over USB.

If the keyboard appears as a removable drive named `NICENANO`, it is in
bootloader mode instead of normal keyboard mode. Flash firmware or unplug and
restart the controller.

### Bluetooth

This keymap includes Bluetooth controls on the lower layer:

```dts
&bt BT_CLR &bt BT_SEL 0 &bt BT_SEL 1 &bt BT_SEL 2 &bt BT_SEL 3 &bt BT_SEL 4
```

Hold the lower layer key (`&mo 1`) and use these keys to select or clear
Bluetooth profiles.

ZMK profile numbers are `0` through `4`. These correspond to five saved host
slots, commonly thought of as Bluetooth profiles 1 through 5.

On the current keymap, the Bluetooth controls are on the left home row while
the lower layer is held:

| Physical key | Lower-layer action |
| --- | --- |
| Left `Ctrl` position | Clear Bluetooth profile |
| `A` position | Select profile 1 |
| `S` position | Select profile 2 |
| `D` position | Select profile 3 |
| `F` position | Select profile 4 |
| `G` position | Select profile 5 |

To pair with a new device:

1. Select the Bluetooth profile you want to use.
2. Open Bluetooth settings on your computer, phone, or tablet.
3. Pair with `Corne View`.
4. If pairing fails, clear the selected profile with `BT_CLR`, remove the old
   keyboard entry from the host, and pair again.

## OLED Status

This firmware enables ZMK display support and several OLED widgets:

```dts
CONFIG_ZMK_DISPLAY=y
CONFIG_ZMK_WIDGET_OUTPUT_STATUS=y
CONFIG_ZMK_WIDGET_LAYER_STATUS=y
CONFIG_ZMK_WIDGET_WPM_STATUS=y
CONFIG_ZMK_WIDGET_BATTERY_STATUS=y
CONFIG_ZMK_WIDGET_BATTERY_STATUS_SHOW_PERCENTAGE=y
```

If your keyboard has OLED displays installed and wired correctly, they can show
status such as output mode, active layer, typing speed, and battery percentage.

If your keyboard does not have OLED displays, these settings do not change the
typing behavior.

## Default Keymap

The following tables summarize the current keymap from
`config/corne.keymap`. The actual `bindings` entries control the firmware, so
if the comment diagram and the bindings ever differ, trust the bindings.

### Default Layer

| Left hand | Right hand |
| --- | --- |
| `TAB` `Q` `W` `E` `R` `T` | `Y` `U` `I` `O` `P` `BSPC` |
| `LCTRL` `A` `S` `D` `F` `G` | `H` `J` `K` `L` `;` `'` |
| `LSHFT` `Z` `X` `C` `V` `B` | `N` `M` `,` `.` `/` `ESC` |
| `LGUI` `LOWER` `SPACE` | `A` `RAISE` `RALT` |

### Lower Layer

Hold the lower thumb key to access numbers, Bluetooth controls, and arrow keys.

| Left hand | Right hand |
| --- | --- |
| `TAB` `1` `2` `3` `4` `5` | `6` `7` `8` `9` `0` `BSPC` |
| `BT_CLR` `BT1` `BT2` `BT3` `BT4` `BT5` | `LEFT` `DOWN` `UP` `RIGHT` `TRANS` `TRANS` |
| `LSHFT` `TRANS` `TRANS` `TRANS` `TRANS` `TRANS` | `TRANS` `TRANS` `TRANS` `TRANS` `TRANS` `TRANS` |
| `LGUI` `TRANS` `SPACE` | `RET` `TRANS` `RALT` |

### Raise Layer

Hold the raise thumb key to access symbols and punctuation.

| Left hand | Right hand |
| --- | --- |
| `TAB` `EXCL` `AT` `HASH` `DLLR` `PRCNT` | `CARET` `AMPS` `ASTRK` `LPAR` `RPAR` `BSPC` |
| `LCTRL` `TRANS` `TRANS` `TRANS` `TRANS` `TRANS` | `MINUS` `EQUAL` `LBKT` `RBKT` `BSLH` `GRAVE` |
| `LSHFT` `TRANS` `TRANS` `TRANS` `TRANS` `TRANS` | `UNDER` `PLUS` `LBRC` `RBRC` `PIPE` `TILDE` |
| `LGUI` `TRANS` `SPACE` | `RET` `TRANS` `RALT` |

## Firmware Updates

Use the GitHub Actions build output for firmware files.

1. Push your changes to GitHub.
2. Open the repository on GitHub.
3. Go to the **Actions** tab.
4. Open the latest successful workflow run.
5. Download and unzip the firmware artifact.
6. Flash the matching `.uf2` file to each half.

See `docs/how-to-flash-firmware.md` for the full flashing guide.

## Keymap Changes

The keymap is stored in:

```text
config/corne.keymap
```

This repository uses source-controlled ZMK keymap editing. ZMK Studio is not
enabled by the current configuration, so keymap changes should be made in the
keymap file, committed, pushed, built by GitHub Actions, and flashed.

See `docs/how-to-edit-keymap.md` for examples.

## Resetting Saved ZMK Settings

Use the `settings_reset` firmware when Bluetooth pairings or split-half
connections are stuck in a bad state.

For each half:

1. Flash the `settings_reset` `.uf2` file.
2. Wait for the controller to reboot.
3. Enter bootloader mode again.
4. Flash the normal left or right `.uf2` firmware.

After resetting both halves, remove old `Corne View` Bluetooth entries from
your host devices and pair again.

## Troubleshooting

### The keyboard does not respond

Check that both halves are powered on and have enough battery. If you are using
USB, confirm that the cable supports data. If only one half responds, check the
split connection steps below.

### The keyboard appears as `NICENANO`

The controller is in bootloader mode. If you intended to flash firmware, copy
the correct `.uf2` file to the `NICENANO` drive. If you wanted normal typing,
unplug the keyboard and reconnect it, or press reset once to leave bootloader
mode.

### The `NICENANO` drive does not appear

Use a USB-C data cable and connect directly to the controller. Double-press the
keyboard reset button quickly to enter bootloader mode. Try a different USB
port if the drive still does not appear.

### USB typing does not work

Make sure the USB cable is connected to the left half. A peripheral half may
still charge or enter bootloader mode over USB, but it is not meant to act as a
standalone full keyboard.

### Bluetooth will not pair

Select the Bluetooth profile you want to use, clear it with `BT_CLR`, and delete
any old `Corne View` entry from the host device. Then pair again from the host's
Bluetooth settings.

If pairing still fails, flash `settings_reset` to both halves, flash normal
firmware again, and remove old host-side Bluetooth entries before pairing.

### Bluetooth is connected but keys do not type

You may be on a different Bluetooth profile than the one paired to the current
computer. Select the correct profile with the lower-layer Bluetooth profile
keys, or clear and re-pair the profile.

If USB is connected at the same time, verify which output mode the keyboard is
using. The OLED output status widget can help if your keyboard has displays.

### One half does not work

Check that both halves are powered on and flashed with matching firmware from
the same build. The left half should use the `corne_left` firmware, and the
right half should use the `corne_right` firmware.

If the halves still do not communicate, flash `settings_reset` to both halves,
then flash the normal firmware to both halves again.

### The halves disconnect frequently

Keep the halves close together during testing. Check battery level on both
halves and move away from heavy wireless interference. If your desktop Bluetooth
adapter uses an external antenna, make sure the antenna is installed and placed
where it has a clear signal.

If the issue started after a firmware change, rebuild and flash both halves
again from the same GitHub Actions artifact.

### Battery drains faster on one half

This can be normal for the central half because it handles host communication
and split communication. If battery life is much shorter than expected, charge
both halves fully and check whether displays, LEDs, or external power features
are consuming power.

RGB underglow is not enabled in this repository by default.

### OLED is blank

Confirm that the keyboard actually has OLED displays installed. If it does,
check the display socket, soldering, orientation, and battery level. The
firmware has display support enabled, so a blank display is usually caused by
hardware connection, display compatibility, or power issues.

### A specific key does not trigger

Test the same physical switch in another socket if the keyboard is hot-swap.
Check for bent switch pins, a misaligned switch, or a damaged socket. If the
key triggers the wrong output, inspect `config/corne.keymap` and make sure the
correct firmware was flashed after the change.

### A keymap change did not appear after flashing

Confirm that you downloaded the artifact from the latest successful GitHub
Actions run. Flash both halves with the newly built firmware, and make sure the
left and right `.uf2` files were not swapped.

### GitHub Actions build fails

Check `config/corne.keymap` first. Every layer should keep the same number of
bindings, and every binding should use a valid ZMK behavior such as `&kp`,
`&mo`, `&trans`, or `&bt`.

Also check that `config/corne.conf` contains valid ZMK configuration symbols
and that no comments were accidentally placed inside a `bindings = < ... >;`
block.

### Bluetooth signal is weak

Try a different host device or Bluetooth adapter. Keep the keyboard away from
USB 3 hubs, metal cases, and crowded 2.4 GHz devices during testing. If using a
desktop PC, install and position the Bluetooth antenna.

This repository already enables higher controller transmit power:

```dts
CONFIG_BT_CTLR_TX_PWR_PLUS_8=y
```

If signal quality is still poor, reset Bluetooth settings and re-pair before
assuming the hardware is faulty.
