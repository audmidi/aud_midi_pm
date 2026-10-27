# aud_midi_pm

Project management repository of the Audanika MIDI packages: goals, requirements, plans, decisions and architecture. No code.

Current quarter: [2026-Q4](doc/2026-Q4).

## Goals

All goals are active since 2026-Q4.

- [One MIDI API for every platform](doc/goals/one-midi-api-for-every-platform.md)
- [Timing a busy app cannot disturb](doc/goals/timing-a-busy-app-cannot-disturb.md)
- [MIDI 2.0 without a rewrite](doc/goals/midi-2-without-a-rewrite.md)
- [Building blocks for other MIDI software](doc/goals/building-blocks-for-other-midi-software.md)
- [The app is a MIDI device](doc/goals/the-app-is-a-midi-device.md)
- [Apps know which notes sound](doc/goals/apps-know-which-notes-sound.md)
- [Verified on real devices](doc/goals/verified-on-real-devices.md)

## Requirements

One file per area, IDs `req-<area>-<nnn>`.

- [API](doc/requirements/api.md): the app-facing API of `aud_midi`
- [Messages](doc/requirements/messages.md): typed messages, codecs, translation and constants of `aud_midi_standard`
- [Engine](doc/requirements/engine.md): threading, timing, diagnostics and the backend contract of `aud_midi_core`
- [Ports](doc/requirements/ports.md): hotplug, identity, virtual ports and capabilities
- [Network](doc/requirements/network.md): AppleMIDI sessions, the RTP-MIDI journal, discovery and Network MIDI 2.0
- [Bluetooth](doc/requirements/bluetooth.md): the app as a Bluetooth LE central and peripheral
- [Platforms](doc/requirements/platforms.md): what each operating system offers and from which version on
- [State](doc/requirements/state.md): the note matrix and the controller matrix of `aud_midi_state_matrix`
- [Constraints](doc/requirements/constraints.md): fixed technologies, package rules and quality gates

## Quarter 2026-Q4

- [Tickets](doc/2026-Q4/tickets)
- [Decisions](doc/2026-Q4/concepts/decisions/000-index.md)
- [Architecture](doc/2026-Q4/architecture/architecture.md)
- [Scenarios: the people and the main scenarios of the family](doc/2026-Q4/concepts/topics/scenarios.md)
- [Plan aud_midi_01: initial MIDI implementation](doc/2026-Q4/tickets/2026-10-06-aud_midi_01-initial-midi-implementation.md)
- [Plan aud_midi_02: track active notes](doc/2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md)
- [References: specifications and reference implementations](doc/references/README.md)
