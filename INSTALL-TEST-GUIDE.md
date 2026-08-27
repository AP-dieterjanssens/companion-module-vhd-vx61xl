# VHD VX61XL / V60 Companion module — install & test guide

This module is for the VHD V60 family: VX61XL, V60, VX60ASL, VX61ASL,
VX61BASL, VX751BA, VX752A, V60XL, V61XL, V63XL, V60XS, V61XS, V63XS.
(It is a separate module from the VHD VX-90 module — the two camera
families share most of their VISCA protocol, but not all of it, so
each module uses the byte encoding actually documented for its own
camera.)

Built from VHD's official "Command list EN VHD V60" VISCA table
(received via Li, 27/08/2026). Uses Companion module API 1.14.1, same
compatibility basis as the VX-90 module — confirmed to work on
Companion 5.0.4/5.1 without any v2 API migration.

## 1. Install

1. Open the Companion web admin UI (usually `http://127.0.0.1:8000`).
2. Go to **Modules** → **Import module package**.
3. Select `vhd-vx61xl-1.0.0.tgz`.
4. Companion installs it immediately — no restart needed.

## 2. Add the camera as a connection

1. **Connections** → **Add connection** → search for "VX61XL" or "V60".
2. Fill in the camera's IP address, VISCA TCP port (default 5678), and
   VISCA camera address (default 1).
3. Save — status should turn green ("Connected") within a few seconds.

## 3. First test

- **Pan/Tilt: Up / Down / Left / Right**, **Zoom: Tele/Wide (standard
  speed)**, **Preset: Recall** — these are byte-identical to the VX-90
  module and were already confirmed working there.
- Then try **Gain (direct)**, **Bright (direct)**, **Color Temperature
  (direct)** — these use the wider/different byte encoding documented
  specifically for this camera family (not the same bytes as the VX-90
  module's equivalents). Worth confirming these behave as expected on
  a real unit, since they haven't been tested against physical
  hardware yet.

## 4. What's different from the VX-90 module

- **Not included**: VHD's VX-90-only extensions (ND filter, EIS, tally
  set/brightness, image style, focus lock, audio input, motion sync,
  extended tracking zoom) — these are not documented for this camera
  family, so including them would just send commands the camera won't
  recognize.
- **Included here but not on the VX-90 module**: NR2D, AWB Sensitivity,
  Infrared Remote On/Off, DVI/HDMI output mode, Save Current Setting,
  Pan/Tilt Relative Position, Pan/Tilt Limit Clear, Voice Tracking,
  Sharpness Auto — all documented in the V60 table but not in the
  VX-90 one.
- **Different byte encoding** (same function, different data bytes):
  Gain, Bright, Brightness, Contrast, Sharpness/Aperture direct values
  use a finer 2-byte encoding on this camera family vs. the VX-90's
  1-byte encoding; Color Temperature direct uses a single raw byte
  here vs. 2 bytes on the VX-90; preset numbers go 0-127 here vs.
  0-254 on the VX-90.

## 5. Troubleshooting

Same as the VX-90 module: check Companion's Log tab for `TX:`/`RX:`
hex lines to confirm what's actually being sent, and check the
connection status if nothing happens.
