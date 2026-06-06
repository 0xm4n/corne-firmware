# How to Edit the Corne Keymap

This repository stores the keyboard layout in:

```text
config/corne.keymap
```

Edit this file when you want to change keys, layers, Bluetooth controls, or
thumb key behavior.

## Keymap Structure

The keymap is a ZMK devicetree file. The important section is:

```dts
/ {
        keymap {
                compatible = "zmk,keymap";

                default_layer {
                        bindings = < ... >;
                };

                lower_layer {
                        bindings = < ... >;
                };

                raise_layer {
                        bindings = < ... >;
                };
        };
};
```

This repository currently has three layers:

- `default_layer`: the normal typing layer.
- `lower_layer`: activated by holding `&mo 1`.
- `raise_layer`: activated by holding `&mo 2`.

Layer numbers are based on order in the file:

- `0`: `default_layer`
- `1`: `lower_layer`
- `2`: `raise_layer`

## Read the Layout

Each layer has a comment block showing the physical key positions, followed by
the real `bindings` block. The `bindings` block is what ZMK actually builds.

For this Corne layout, each layer has 42 bindings:

- 12 keys on the top row.
- 12 keys on the home row.
- 12 keys on the bottom row.
- 6 thumb keys.

Keep the same number of bindings on every layer. If a layer has too many or too
few entries, the firmware build will fail or the layout will not match the
keyboard.

## Common Binding Types

### Normal Key Press

Use `&kp` for a normal key press:

```dts
&kp Q
&kp SPACE
&kp TAB
&kp BSPC
```

To change a key, replace only the key code after `&kp`.

Example:

```dts
&kp Q
```

can become:

```dts
&kp ESC
```

### Momentary Layer

Use `&mo` to activate a layer while the key is held:

```dts
&mo 1
&mo 2
```

In this keymap:

- `&mo 1` holds the lower layer.
- `&mo 2` holds the raise layer.

### Transparent Key

Use `&trans` when a key should fall through to the same position on a lower
layer:

```dts
&trans
```

This is useful on the lower and raise layers when you want a key to behave like
it does on the default layer.

### Bluetooth Controls

This keymap uses ZMK Bluetooth behaviors on the lower layer:

```dts
&bt BT_CLR
&bt BT_SEL 0
&bt BT_SEL 1
&bt BT_SEL 2
&bt BT_SEL 3
&bt BT_SEL 4
```

Use `BT_SEL` to switch Bluetooth profiles. Use `BT_CLR` to clear Bluetooth
pairing information.

## Example: Change a Letter Key

Find the key in the `default_layer` bindings.

For example, this part of the top row:

```dts
&kp TAB   &kp Q &kp W &kp E &kp R &kp T
```

To change `Q` to `ESC`, edit it to:

```dts
&kp TAB   &kp ESC &kp W &kp E &kp R &kp T
```

After changing the binding, update the comment block above the layer so the
visual layout stays accurate.

## Example: Change a Thumb Key

The thumb keys are the last 6 bindings in each layer.

In the default layer they currently look like this:

```dts
&kp LGUI &mo 1 &kp SPACE   &kp A &mo 2 &kp RALT
```

For example, to make the first right thumb key send Enter, change:

```dts
&kp A
```

to:

```dts
&kp RET
```

## Example: Add a Key to a Layer

On the lower and raise layers, replace an existing `&trans` entry with the key
you want.

Example:

```dts
&trans
```

can become:

```dts
&kp HOME
```

The key will only send `HOME` while that layer is active.

## Key Code Names

This file includes ZMK key code definitions at the top:

```dts
#include <dt-bindings/zmk/keys.h>
```

Use ZMK key names such as:

- Letters: `A`, `B`, `C`
- Numbers: `N1`, `N2`, `N3`
- Modifiers: `LCTRL`, `LSHFT`, `LGUI`, `RALT`
- Navigation: `LEFT`, `DOWN`, `UP`, `RIGHT`, `HOME`, `END`
- Symbols: `MINUS`, `EQUAL`, `LBKT`, `RBKT`, `BSLH`, `GRAVE`

When in doubt, use the same naming style already present in
`config/corne.keymap`.

## Build the Firmware

After editing `config/corne.keymap`:

1. Commit and push your change to GitHub.
2. Open the repository on GitHub.
3. Go to the **Actions** tab.
4. Wait for the build workflow to finish.
5. Download the generated firmware artifact.
6. Flash the new `.uf2` files onto the keyboard.

See `docs/how-to-flash-firmware.md` for the flashing steps.

## Troubleshooting

If the GitHub Actions build fails:

- Check that every layer still has exactly 42 bindings.
- Check that every binding starts with a valid behavior, such as `&kp`, `&mo`,
  `&trans`, or `&bt`.
- Check that key names are spelled correctly.
- Make sure every `bindings = < ... >;` block still has both `<` and `>;`.
- Make sure comments and bindings were not accidentally mixed together.
