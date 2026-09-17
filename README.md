# Lumi MacroPad ZMK

Project for the user's nice!nano-compatible nRF52840 macropad:

- 12 matrix keys
- EC11 encoder + push switch
- Bluetooth HID
- USB HID
- ZMK Studio remapping
- Optional 1.47" 172x320 ST7789 TFT build

## Physical wiring

### Matrix
Rows:
- `009` -> P0.09
- `010` -> P0.10
- `111` -> P1.11
- `113` -> P1.13

Columns:
- `115` -> P1.15
- `002` -> P0.02
- `029` -> P0.29
- `031` -> P0.31

Diodes are `COL -> switch -> diode -> ROW`, with the black stripe/cathode toward the row.

Encoder:
- A -> `104` / P1.04
- B -> `106` / P1.06
- C -> GND
- Encoder push -> matrix ROW0/COL3 (`031 -> switch -> diode -> 009`)

### TFT
- SDA -> `017` / P0.17
- SCL -> `020` / P0.20
- CS -> `022` / P0.22
- DC -> `024` / P0.24
- RES -> `100` / P1.00
- VDD -> VCC
- BL -> VCC
- GND -> GND

## Build outputs

`build.yaml` builds three artifacts:

1. `lumi_macropad_studio`
   - Recommended first.
   - ZMK Studio + BLE/USB + keys + encoder.
   - No TFT.

2. `lumi_macropad_studio_tft`
   - Same keyboard + ZMK Studio.
   - Enables ST7789 and ZMK's built-in status display.
   - TFT support is more experimental than the base build because Zephyr's ST7789/MIPI-DBI stack has changed recently.

3. `settings_reset`
   - Use only when you need to clear ZMK Bluetooth/settings storage.

## Default keymap

There are three layers: Office, Media, Fusion 360.

The encoder push cycles Office -> Media -> Fusion 360 -> Office by default.

Encoder rotation:
- Office: volume up/down
- Media: next/previous track
- Fusion 360: page up/down

The 13 key positions (12 keys + encoder push) are represented in the physical layout, so ZMK Studio can remap them.

Encoder rotation is configured in firmware using `sensor-bindings`; upstream ZMK Studio does not currently remap encoder rotation.

## How to build on GitHub

1. Create a new GitHub repository.
2. Upload the contents of this folder to the repository root.
3. Open the repository's **Actions** tab.
4. Run/wait for **Build ZMK firmware**.
5. Download the firmware artifact.
6. Double-reset nice!nano so the `NICENANO` drive appears.
7. Copy the desired `.uf2` to the drive.

Start with `lumi_macropad_studio`. After Studio/keys/encoder are confirmed, try the TFT artifact.

## ZMK Studio

Use the official ZMK Studio web app after flashing the Studio build.
Connect the macropad by USB for the most reliable Studio RPC connection.

The key assignments changed in Studio are stored on the keyboard, so normal key changes do not require rebuilding firmware.
