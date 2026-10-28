# mib2emu — MIB2 STD head unit on a PC

Runs the **real HMI of a MIB2 STD unit** (Technisat MST2, SW 0480, Škoda STD2 Nav, HMI H29.319.120) on a Windows PC. It loads the Java code, skins, fonts and views , plus the long coding. Nothing from the car's QNX side runs — the backend (startup, power, persistence, car data, radio, phone, media…) is re-implemented in Java and answers the unmodified HMI exactly like the ECUs would.


![Screenshot](Screenshots/screenshot_1793052569574.png)



> **This is our code only.** The HMI firmware, skins, boot animations and vehicle coding are the car maker's (Technisat / VW / Škoda /VW AG) . Free to **use, share and promote** — but **not** to modify or sell (see `LICENSE`); and the license covers our source and tooling only, not any firmware or dump.

## Download & run

Install **JDK 21** (Eclipse Temurin 21 from adoptium.net). It must be **21 exactly** — this build uses the Java 21 foreign-function API; 22+ won't run it.

Install [FFmpeg](https://www.ffmpeg.org/) and add it to PATH(Required for the radio to play music)

2. ### Downloading the Repository

Git LFS is required to download all large files correctly.

1. Install [Git](https://git-scm.com/downloads) and [Git LFS](https://git-lfs.com/).

 Open a terminal and run:

   ```bash
   git lfs install
   git clone https://github.com/JEtsKi8271/mibstd2-emulator.git
   ```

3. Enter the repository folder and ensure all LFS files are downloaded:

   ```bash
   cd mibstd2-emulator
   git lfs pull
   ```

**Important:** Do not download the repository using GitHub's **Download ZIP** option, as large Git LFS files may not be included as expected.

If some files are unavailable through Git LFS, use the alternative download links below.

**Alternative download:** [Google Drive](https://drive.google.com/file/d/1NENlX9aBG0JQbOrLG1hq4mCa8eLY3yME/view?usp=sharing)

3. Double-click **`run.bat`**.





The window opens with the unit's **boot animation** ("Welcome to ŠKODA"), and the HMI takes over once it's ready, like in the car. It's 800×480 shown at 1.5×. A few menus fill in only after ~80 s because some simulated services still answer certain startup steps by timeout.

---

## What works

### Display & input
- The complete, unmodified MIB2 STD HMI, rendered through ANGLE (same GL ES 2 API as the unit's Vivante GPU), composited into a Swing window.
- Touchscreen via mouse (click + drag), all hard keys, screenshots.
- Boot animations — all 14 Škoda variants on the unit (Welcome, RS, Scout, Laurin & Klement, Greenline, Monte-Carlo, SportLine…), selectable.

### Menus & car
- MENU / RADIO / MEDIA / PHONE / NAV* / CAR / SETUP / TRAFFIC.
- **Driving modes** — Normal → Sport → Eco → Comfort → Individual → Offroad popup; hill-descent control only in Offroad, as in the car.
- **Trip computer** — consumption, range, fuel, trip, odometer (live-editable from the web UI).
- **Vehicle settings** echoed back and remembered like the ECUs: ESC mode, HDC, ambient lighting (all zones, 10 colours from the skin's table — the chosen colour also tints the HMI), etc.
- **Driver profiles** — rename them, and switching a profile restores that profile's saved driving mode + volume.
- **Status-bar clock** — pulled from the PC's local time and date.
- Camera views (rear / Area View / additional) draw photos from the `camera/` folder into the HMI's transparent video area; next photo on each open.

### Radio
- FM tuner plays **Radio Garden** internet stations: each entry in `radio-garden.txt` is an FM station on the MIB — shows in the list, tunes via list / presets / ◀ ▶ seek / manual. Add stations by searching Radio Garden from the web UI. (Needs `ffmpeg` on PATH.)

### Phone & Bluetooth
One link, because Windows can't act as a hands-free car kit:
- **ADB** (Android over USB / wireless debugging): phone name, battery, signal, call history, all contacts, and calls. Dial from keypad / contact / call list, answer, hang up — all act on the real phone (call audio stays on the phone).


### Media
- **USB**: your Music folder is a USB stick — browse folders, play/pause, next/prev, seek, repeat/shuffle, tag info (ffprobe), progress. Played via ffmpeg.
- Radio and media take turns like in the car; the active source mutes the other.

### Android Auto
Real Android Auto from your phone on the MIB screen, via Google's **Desktop Head Unit**:
1. Phone on ADB. 2. On the phone: AA settings → tap *Version* 10× → ⋮ → **Start head unit server**. 3. Start the emulator → SmartLink lists the phone as *android auto* → tap it.
The DHU runs hidden; its picture is drawn in the MIB's AA screen, MIB touches go back to it, AA audio plays through it. The emulator codes AA on (adaptation 1142, off on this unit); `-Dmib2pc.androidauto=false` keeps the original, `-Dmib2pc.aa.fake=true` tests without a phone. **Exit** returns to the SmartLink device list.

### Web UI
Opens in your browser automatically on start (`-Dmib2pc.web.open=false` to stop). Also at **http://localhost:8080/** (localhost only): live screen (click/drag = touch), hard keys, driving-mode buttons, trip-computer sliders, volume, next camera photo, radio station list, startup-animation picker, and a **Restart head unit** button. JSON API is documented atop `src/mib2pc/web/Web.java`, e.g. `curl -X POST "localhost:8080/api/mode?profile=3"`.

---

## Controls

| Input | MIB | Input | MIB |
|---|---|---|---|
| Click / drag | Touchscreen | `C` | CAR |
| `M` / `Home` | MENU | `S` | SETUP |
| `R` | RADIO | `T` | TRAFFIC |
| `E` | MEDIA | `D` | MODE (driving-mode popup) |
| `P` | PHONE | `←` / `→` | Prev / next |
| `N` | NAV | `Esc` / `Backspace` | BACK |
| | | `F12` | Screenshot → HMI `snapshots/` |


## Known gaps

- **No map, browser, CarPlay or video playback** — those run as QNX processes on the real unit. (Android Auto works, via the DHU.)

## Credits & license

Built by **JEtsKi (JEtsKi8271) and Razvan2 (Razvandxv) — The Cluster Hub**.

Free to **use, share, and promote** with proper credit. **Modification, redistribution of modified versions, and commercial use are prohibited** under the terms of the `LICENSE`.

**No Reuse of Project Files:** You may not use, copy, extract, incorporate, or reuse any files from this project in your own projects, repositories, tools, or products. This includes source code, scripts, firmware files, assets, configurations, and other included materials.

These restrictions apply to our source code, files, and tooling only, as specified in the `LICENSE`.

![Screenshot](Screenshots/screenshot_1793052580346.png)
![Screenshot](Screenshots/screenshot_1793052586943.png)
![Screenshot](Screenshots/screenshot_1793052591119.png)
![Screenshot](Screenshots/screenshot_1793052593851.png)
![Screenshot](Screenshots/screenshot_1793052601033.png)
![Screenshot](Screenshots/screenshot_1793052603111.png)
![Screenshot](Screenshots/screenshot_1793052610919.png)
![Screenshot](Screenshots/screenshot_1793052618474.png)
![Screenshot](Screenshots/screenshot_1793052632487.png)
![Screenshot](Screenshots/screenshot_1793052640434.png)
![Screenshot](Screenshots/screenshot_1793052642677.png)
![Screenshot](Screenshots/screenshot_1793052649752.png)
![Screenshot](Screenshots/screenshot_1793052655136.png)
