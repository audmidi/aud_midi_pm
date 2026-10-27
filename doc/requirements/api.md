# API

What the app-facing API of `aud_midi` must do. Accepted with the user on
2026-10-27 in ticket aud_midi_03.

## req-api-001: One call opens MIDI on every platform

- Status: accepted
- Priority: must
- Statement: An app opens MIDI with one call and gets the inputs and
  outputs of the platform it runs on: iOS, macOS, Android, Windows, Linux
  and the web
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: `hello_midi` runs unchanged on all six platforms and lists
  every port the operating system shows

## req-api-002: Ports are the primary entity

- Status: accepted
- Priority: must
- Statement: Every input and output is a port with a name, its device,
  its transport (USB, Bluetooth LE, virtual, network), its protocol
  (MIDI 1.0 or UMP) and its capabilities
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: the example app shows name, device, transport, protocol and
  capabilities of every port on every platform

## req-api-003: Receive with timestamps

- Status: accepted
- Priority: must
- Statement: An app subscribes to an input and receives its messages,
  typed or raw, in order and each with the time of its arrival on the
  package clock
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - a Note On sent through a loopback arrives as `MidiNoteOn` with a
    timestamp
  - raw packets of the same input carry the same timestamps

## req-api-004: Send now or at a due time

- Status: accepted
- Priority: must
- Statement: An app sends typed messages or raw packets to an output at
  once or for a due time on the package clock
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy
- Acceptance: `hello_midi` plays C4 at once and releases it 500 ms later
  through a send with a due time; the Note Off arrives 500 ms after the
  Note On within the tolerance of req-engine-003

## req-api-005: One dependency

- Status: accepted
- Priority: must
- Statement: An app depends on `aud_midi` alone to use messages, ports
  and the backends of all platforms
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: the example app lists `aud_midi` as its only MIDI
  dependency and imports nothing else of the family

## req-api-006: Other isolates attach

- Status: accepted
- Priority: should
- Statement: Any isolate of the app, a worker next to the UI, uses MIDI
  through the same API as the main isolate
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy;
  decision isolate-001
- Acceptance: a test attaches a second isolate with the connector and
  sends and receives through it

## req-api-007: Close releases everything

- Status: accepted
- Priority: must
- Statement: Closing MIDI releases every port, virtual port, session and
  native resource, and opening MIDI again works
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, appear as a virtual port
- Acceptance:
  - after close, a virtual port of the app is gone from other apps
  - a test opens and closes MIDI ten times in one process without a
    crash or a leaked port

## req-api-008: Platform setup documented

- Status: accepted
- Priority: must
- Statement: The permissions, entitlements, manifest entries and
  capabilities an app needs on each platform are documented, and the
  example app carries them
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, connect a Bluetooth LE keyboard
- Acceptance:
  - the README of `aud_midi` lists the setup per platform
  - the example app builds with that setup on every platform
