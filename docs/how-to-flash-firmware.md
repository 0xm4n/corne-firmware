# How to Flash Corne nice!nano Firmware

This guide explains how to flash the ZMK `.uf2` firmware files for this Corne
configuration onto nice!nano v2 controllers.

## Before You Start

You need:

- A Corne keyboard with nice!nano v2 controllers.
- A USB data cable that supports data transfer, not just charging.
- The `.uf2` firmware files for the left and right halves.

## Get the Firmware Files

If you cloned this repository from
`https://github.com/0xm4n/corne-firmware`, you can download prebuilt firmware
from GitHub Actions:

1. Open the repository on GitHub.
2. Go to the **Actions** tab.
3. Select the latest successful workflow run.
4. Download the firmware artifact from that run.
5. Unzip the artifact on your computer.

This repository builds firmware for:

- `corne_left` on `nice_nano_v2`
- `corne_right` on `nice_nano_v2`
- `settings_reset` on `nice_nano_v2`

Use the left firmware for the left half and the right firmware for the right
half. The settings reset firmware is optional and is only needed when you want
to clear the controller's saved ZMK settings, such as Bluetooth pairings.

## Connect One Half

Flash one half at a time.

1. Connect one keyboard half to your computer with the USB cable.
2. Make sure the cable is connected directly to the nice!nano USB-C port.

## Enter Bootloader Mode

The nice!nano must be in bootloader mode before it can accept a `.uf2` file.

Use the keyboard reset button and press it twice quickly. The controller should
reboot into bootloader mode.

When bootloader mode is active, your computer should show a removable USB drive
named `NICENANO`.

## Flash the Firmware

1. Locate the correct `.uf2` file for the half you connected.
2. Copy or drag that `.uf2` file onto the `NICENANO` USB drive.
3. Wait for the copy to finish.
4. The nice!nano will automatically flash the firmware and disconnect.

After the flash completes, safely eject the drive if it is still visible, then
unplug the USB cable.

## Flash the Other Half

Repeat the same process for the other half:

1. Connect the other half over USB.
2. Enter bootloader mode.
3. Copy the matching `.uf2` file to the `NICENANO` drive.
4. Wait for the controller to flash and reboot.

## Optional: Reset ZMK Settings

If the keyboard has stale Bluetooth pairings or behaves unexpectedly after a
firmware change, flash the `settings_reset` firmware to each half once:

1. Put the half into bootloader mode.
2. Copy the `settings_reset` `.uf2` file to the `NICENANO` drive.
3. Wait for the controller to reboot.
4. Put the half back into bootloader mode.
5. Flash the normal left or right firmware again.

## After Flashing

After both halves have the correct firmware:

1. Power on both halves.
2. Connect the central half to your computer over USB or pair it over Bluetooth.
3. If using Bluetooth, pair the keyboard from your operating system's Bluetooth
   settings.

For a split ZMK keyboard, only the central half connects to the computer. The
other half communicates with the central half wirelessly.
