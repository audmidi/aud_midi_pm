# Platforms

What the family must offer on each operating system and from which
version on. Accepted with the user on 2026-10-27 in ticket aud_midi_03.
The backends behind it are decisions, see the architecture.

## req-platforms-001: iOS and macOS

- Status: accepted
- Priority: must
- Statement: On iOS 14 and macOS 11 and later the app uses USB,
  Bluetooth LE, virtual ports created at runtime and network sessions,
  with native UMP from these versions on and UMP endpoints from iOS 18
  and macOS 15 on
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, plug in a USB keyboard; decision apple-001
- Acceptance: the example app on an iPhone or iPad and on a Mac plays a
  USB keyboard and a Bluetooth LE keyboard, shows a virtual port to
  another app and joins a session of Audio MIDI Setup

## req-platforms-002: Android

- Status: accepted
- Priority: must
- Statement: On Android 7 (API 24) and later the app uses USB and
  Bluetooth LE MIDI, virtual devices declared in the manifest and
  network sessions; sends with due times from Android 10 (API 29) on,
  UMP from Android 13 (API 33) on
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - the example app in a release build with R8 plays a USB keyboard and
    a Bluetooth LE keyboard on a phone
  - the loopback tests pass on the emulator

## req-platforms-003: Windows

- Status: accepted
- Priority: must
- Statement: On Windows 10 and later the app uses USB and paired
  Bluetooth LE MIDI and network sessions; where Windows MIDI Services
  are present, also UMP, virtual devices and Bluetooth LE pairing from
  the app
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - the example app built and run on a Windows machine plays a USB
    keyboard
  - with Windows MIDI Services installed, the app detects them at
    runtime and shows a virtual device to another app

## req-platforms-004: Linux

- Status: accepted
- Priority: must
- Statement: On Linux with the ALSA sequencer the app uses the ports of
  the sequencer, USB, other apps and the PipeWire and JACK bridges,
  virtual ports created at runtime, Bluetooth LE over BlueZ and network
  sessions advertised over Avahi; UMP on kernel 6.5 with alsa-lib 1.2.10
  and later
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - the example app run on a Linux machine plays a USB keyboard
  - a virtual port of the app is seen and used by another ALSA client

## req-platforms-005: Web

- Status: accepted
- Priority: must
- Statement: In browsers with Web MIDI, Chromium-based browsers and
  Firefox, the app uses the ports the operating system exposes, with
  SysEx after the permission of the browser; Bluetooth LE, virtual
  ports, network sessions and UMP are unavailable there
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, plug in a USB keyboard; decision web-001
- Acceptance: the example web app in Chrome and Firefox, compiled with
  dart2js and to Wasm, plays through a CoreMIDI loopback on a Mac

## req-platforms-006: Missing features are visible

- Status: accepted
- Priority: must
- Statement: A feature a platform lacks is reported through the API,
  never by a crash or a silent no-op
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, appear as a virtual port
- Acceptance:
  - on the web the virtual port, Bluetooth and network parts of the API
    are absent and the capabilities say so
  - no call of the API throws because a platform lacks a feature
