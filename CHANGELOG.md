# What's new

## 2026-11-09 — CarPlay, phone and media update

### Added

- **Bundled PlayPort receiver:** the pinned receiver source now ships with the emulator, including our Windows Bluetooth bridge, wired iAP2 transport, viewer changes, OEM icon support and regression tests. First launch builds the server and viewer locally.
- **Wired CarPlay launcher:** restored `mib2emu/carplay-usb.bat`; wireless testing remains available through `carplay.bat`.
- **CarPlay accessory identity:** included the repository owner's supplied `identity.pk8`, `certificate.p7b` and `DiPlay.apk` under `mib2emu/tools/playport/identity`. The key/certificate pair passed the signing validation tests.
- **YouTube Music source:** `mib2emu/youtube-music.bat` opens a dedicated Edge player with a local API. The MIB displays real title, artist, album, artwork, duration and position, with play/pause, next/previous, seek, volume and mute controls. Compatible desktop-player APIs are also supported.
- **Per-phone favourite contacts** with persistence across emulator restarts.
- **Build and test scripts:** `mib2emu/tools/build.ps1`, `tools/sim-tests/check.ps1`, Bluetooth dependency setup and focused CarPlay checks.

### Fixed

- Active-call contact name lookup, including formatted numbers and calls dialled from contact/history entries. Empty numbers no longer match an arbitrary contact.
- Playback races that could restart audio after pausing. Seeking a paused USB track now keeps it paused, and repeated play requests do not rewind it.
- Sound sliders now affect decoded local radio/USB audio; volume controls also reach the Windows Bluetooth audio session.
- Bluetooth endpoint discovery, connection callback/target handling, overlapping profile requests, cancellation and late connection results.
- Bluetooth mute and local pause now gate the received audio even when the phone exposes no remote-control service. Repeated identical unsupported-control warnings are deduplicated.
- The phone contact-detail screen's invalid 1 ms timeout, plus success responses containing missing contact entries.
- Stale Spotify branding when YouTube Music is selected: provider-aware icons and labels, startup provider detection and skin-cache refresh were added.

### Upstream changes retained

- Merged upstream `dd66f00` while preserving the local phone/media improvements.
- DCC/offroad layout selection through `DRIVING_MODES`, additional equipment adaptations and front-seat ventilation availability.
- Aspect-ratio-preserving window rendering with adjusted touch coordinates.

### Setup and current limits

- Use **Git LFS** and run `git lfs pull` after updating so the receiver assets and authentication files are downloaded rather than left as pointers.
- CarPlay requires **Java 21**, **Node.js/npm**, the Windows USB prerequisites described in [CARPLAY.md](mib2emu/CARPLAY.md), and an unlocked/trusted iPhone. The bundled PlayPort code retains its GPL-3.0 license.
- The supplied accessory identity is experimental and can be revoked; it is not an identity issued to this integration. Personal phone pairing records, generated session tokens and live runtime data are not published.
- Working Bluetooth audio does **not** guarantee phone metadata or phone-side next/previous. Those require an accessible media-control service; iPhone Apple Media Service support depends on the available Bluetooth LE connection. Local mute/volume/pause work independently.
- YouTube Music uses the spare desktop-media USB slot while available; Spotify uses that slot when YouTube Music is closed. Sign in and select a track in the dedicated player if needed.
- CarPlay calls, Siri/microphone, wireless handoff and cable reconnect recovery remain experimental. Offline tests do not establish compatibility with every iPhone/iOS version.

### Validation

Passed PlayPort server/protocol tests, viewer tests/build, Windows bridge ABI checks, USB carkit tests, emulator serializer/media regression checks, the headless Edge player API checks, and both accessory identity validation tests.

See [media integration setup](mib2emu/docs/MEDIA_INTEGRATIONS.md) and [phone/Bluetooth log diagnostics](mib2emu/docs/LOG_DIAGNOSTICS.md) for details.
