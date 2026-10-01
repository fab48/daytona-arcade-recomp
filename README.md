ROM is here : https://www.planetemu.net/rom/mame-roms-merged/daytona93


# Daytona USA static recompilation

Daytona USA (Sega Model 2) rebuilt as native code: the game's i960 program
and the TGP program it uploads are statically recompiled to portable C++,
and the fixed-function hardware (geometrizer, rasterizer, tilemaps) is native
C++. No interpreter, no emulation core. MAME is used only as a test oracle.
See `docs/daytona-usa-recomp-design.md` and `HANDOFF.md`.

No game data is in this repository. You need your own `daytona93` ROM set
(a MAME-format `.zip` or `.7z`).

## Setup

**New here? Follow [docs/getting-started.md](docs/getting-started.md)**: step
by step for Windows, macOS and Linux, with fixes for the usual problems.

In short:

1. Copy your `daytona93` ROM set to `roms/daytona93.zip` (or `.7z`) in the
   project folder, with exactly that name. Only the `daytona93` set (Daytona
   USA Deluxe '93) works; other Daytona sets are rejected.
2. Run setup. Linux or macOS:

       ./setup.sh

   Windows (PowerShell):

       powershell -ExecutionPolicy Bypass -File setup.ps1

3. Run the command setup prints at the end (`build/daytona`, or
   `build\Release\daytona.exe` on Windows).

Setup scripts install the toolchain (C++20 compiler, CMake, Ninja, Python 3, Git;
Visual Studio 2022 Build Tools on Windows, Homebrew packages on macOS, your
distribution's packages on Linux), fetch the pinned dependencies into
`extern/`, build, and run the tests. Put your ROM set at
`roms/daytona93.zip` (or `.7z`) first and the game code is recompiled as well (into
`build/`, never committed). Already have a toolchain? Run
`python3 scripts/setup.py` directly.

Options: `--test-extras` (optional test dependencies), `--with-mame` (MAME
source for the oracle test), `--build-mame` (the patched MAME that records
validation traces; Linux and macOS).

After changing the recompiler or the seeds: `python3 scripts/recompile.py`.

## Playing

    build/daytona

(`build/Release/daytona.exe` with the Visual Studio generator.) The launcher
opens first:

- **Game**: choose your `daytona93` ROM set, `.zip` or `.7z` (Browse, or type the path); every file
  is checked against the ROM set this build was recompiled from. Graphics API
  (automatic, Vulkan, Direct3D 12, Metal) and fullscreen. Enhancements
  (off by default): widescreen 16:10, 16:9 or 21:9, which shows more of the
  scene at the sides with the HUD kept 4:3 in the centre, or with "HUD at
  the screen edges" the lap times, position, condition panel and course map
  moved out to the sides. Start.
- **Controls**: bind every arcade control to a key and a gamepad button or
  axis (click, then press). Triggers and sticks are analogue: the accelerator
  and brake follow trigger travel, steering follows the stick. Live meters,
  dead zone, invert steering.

In the game, Esc brings the launcher back (Resume, Reset, Quit). Settings
are saved as they change, with the settings EEPROM and backup RAM, in your
user data folder (`launcher.ini`). Options: `--rom FILE.zip --autostart
--gpu vulkan|direct3d12|metal --fullscreen`. Resolution and upscaling
options are to come.

Default controls:

| Control | Keyboard | Gamepad |
| --- | --- | --- |
| Steer | Left / Right | Left stick (analogue) |
| Accelerate / brake | Up / Down | Right / left trigger (analogue) |
| Gears 1-4 | 1 2 3 4 | |
| View buttons VR1-VR4 | A S D F | Face buttons |
| Shift up / down | W / Q | Right / left shoulder |
| Coin / start | 5 / Enter | Back / Start |
| Test / service | F2 / F3 | |
| Fullscreen / launcher | F11 / Esc | |

Sound: the sound board's 68000 program is statically recompiled like the
i960 code and runs on the native board with the YM3438 (ymfm) and both
MultiPCMs; output goes through SDL audio. Volume and mute are in the
launcher.
