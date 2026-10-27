# One MIDI API for every platform

## Status

active since 2026-Q4

## Outcome

Dart and Flutter apps reach the MIDI devices of their users on iOS,
macOS, Android, Windows, Linux and the web through one API, whatever the
transport: USB, Bluetooth LE, virtual ports of other apps and network
sessions. App developers write their MIDI code once and never touch
platform channels or native code.

## Measure

- The example app of `aud_midi` builds on all six platforms
- `hello_midi` exchanges MIDI with a real or virtual port on each of them
- Every transport of the transport matrix is verified on at least one
  platform with real devices

## Requirements

- [req-api-001: One call opens MIDI on every platform](../requirements/api.md#req-api-001-one-call-opens-midi-on-every-platform)
- [req-api-002: Ports are the primary entity](../requirements/api.md#req-api-002-ports-are-the-primary-entity)
- [req-api-003: Receive with timestamps](../requirements/api.md#req-api-003-receive-with-timestamps)
- [req-api-005: One dependency](../requirements/api.md#req-api-005-one-dependency)
- [req-api-007: Close releases everything](../requirements/api.md#req-api-007-close-releases-everything)
- [req-api-008: Platform setup documented](../requirements/api.md#req-api-008-platform-setup-documented)
- [req-bluetooth-001: The app as a Bluetooth LE central](../requirements/bluetooth.md#req-bluetooth-001-the-app-as-a-bluetooth-le-central)
- [req-bluetooth-004: Reconnect after a dropout](../requirements/bluetooth.md#req-bluetooth-004-reconnect-after-a-dropout)
- [req-constraints-001: No platform channels](../requirements/constraints.md#req-constraints-001-no-platform-channels)
- [req-constraints-006: No server, no telemetry](../requirements/constraints.md#req-constraints-006-no-server-no-telemetry)
- [req-engine-011: Long SysEx](../requirements/engine.md#req-engine-011-long-sysex)
- [req-messages-006: BLE-MIDI framing](../requirements/messages.md#req-messages-006-ble-midi-framing)
- [req-network-001: AppleMIDI sessions](../requirements/network.md#req-network-001-applemidi-sessions)
- [req-network-002: Losses repaired by the journal](../requirements/network.md#req-network-002-losses-repaired-by-the-journal)
- [req-platforms-001: iOS and macOS](../requirements/platforms.md#req-platforms-001-ios-and-macos)
- [req-platforms-002: Android](../requirements/platforms.md#req-platforms-002-android)
- [req-platforms-003: Windows](../requirements/platforms.md#req-platforms-003-windows)
- [req-platforms-004: Linux](../requirements/platforms.md#req-platforms-004-linux)
- [req-platforms-005: Web](../requirements/platforms.md#req-platforms-005-web)
- [req-platforms-006: Missing features are visible](../requirements/platforms.md#req-platforms-006-missing-features-are-visible)
- [req-ports-001: Hotplug within a second](../requirements/ports.md#req-ports-001-hotplug-within-a-second)
- [req-ports-002: The same port after a re-plug](../requirements/ports.md#req-ports-002-the-same-port-after-a-re-plug)
- [req-ports-004: Capabilities per port](../requirements/ports.md#req-ports-004-capabilities-per-port)
