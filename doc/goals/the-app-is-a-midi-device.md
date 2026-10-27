# The app is a MIDI device

## Status

active since 2026-Q4

## Outcome

Other apps and devices see an app built on the family as a MIDI device
of its own: as virtual ports on the same machine, as a session on the
network and as a Bluetooth LE peripheral. A tablet running the app
becomes an instrument or a controller for a DAW, a hardware synth or
another app, without a cable adapter.

## Measure

- A virtual port of the app appears in another app on macOS, iOS,
  Android, Linux and Windows MIDI Services
- A DAW on another machine connects to the app's network session and
  exchanges notes
- A Bluetooth LE host connects to the app and exchanges notes

## Requirements

- [req-bluetooth-002: The app as a Bluetooth LE peripheral](../requirements/bluetooth.md#req-bluetooth-002-the-app-as-a-bluetooth-le-peripheral)
- [req-network-001: AppleMIDI sessions](../requirements/network.md#req-network-001-applemidi-sessions)
- [req-network-003: Discovery and advertising](../requirements/network.md#req-network-003-discovery-and-advertising)
- [req-ports-003: Virtual ports](../requirements/ports.md#req-ports-003-virtual-ports)
