# Ports

What the family must do with ports: hotplug, identity, virtual ports and
capabilities. Accepted with the user on 2026-10-27 in ticket aud_midi_03.

## req-ports-001: Hotplug within a second

- Status: accepted
- Priority: must
- Statement: A device that is connected or removed appears in or vanishes
  from the port list, and the app is notified of it, within 1 s
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - a virtual port created by another app appears in the list within
    1 s on macOS, iOS, Android, Windows, Linux and the web
  - unplugging a USB device removes its ports within 1 s

## req-ports-002: The same port after a re-plug

- Status: accepted
- Priority: must
- Statement: A port that was unplugged and plugged in again is recognised
  as the same port by its fingerprint, while port ids stay unique within
  a session
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard; decision ports-001
- Acceptance:
  - after unplugging and re-plugging a USB device, the fingerprint of the
    new port equals the old one and a consumer bound by fingerprint
    resumes
  - two ports of one session never share an id

## req-ports-003: Virtual ports

- Status: accepted
- Priority: must
- Statement: An app creates named virtual inputs and outputs that other
  apps on the same device see and connect to; at runtime where the
  operating system allows it, declared statically where it does not
- Why: [the app is a MIDI device](../goals/the-app-is-a-midi-device.md),
  topic `scenarios`, appear as a virtual port
- Acceptance:
  - another app lists and uses the virtual port on macOS, iOS, Android,
    Linux and on Windows with Windows MIDI Services
  - on the web the API says that virtual ports are unavailable

## req-ports-004: Capabilities per port

- Status: accepted
- Priority: should
- Statement: Every port says what it supports, protocol, scheduled send,
  SysEx and timestamps, so an app adapts without trying and failing
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, use a MIDI 2.0 device
- Acceptance:
  - the example app shows the capabilities of every port
  - a port of the fake backend without scheduled send reports it, and
    the engine schedules its messages in software
