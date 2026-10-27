# Timing a busy app cannot disturb

## Status

active since 2026-Q4

## Outcome

Musicians hear notes when they are due. Input events keep the time the
operating system stamped, output is scheduled by the operating system
wherever it can be, and all MIDI I/O runs in its own isolate, so frame
building or blocking work in the app never delays it.

## Measure

- `midi_bench` shows a round trip through the MIDI isolate under 1 ms at
  p99
- An OS-scheduled send arrives under 1 ms after its due time at p99
  while the app isolate is busy
- A software-scheduled send arrives under 5 ms after its due time at p99

## Requirements

- [req-api-003: Receive with timestamps](../requirements/api.md#req-api-003-receive-with-timestamps)
- [req-api-004: Send now or at a due time](../requirements/api.md#req-api-004-send-now-or-at-a-due-time)
- [req-api-006: Other isolates attach](../requirements/api.md#req-api-006-other-isolates-attach)
- [req-bluetooth-003: Timestamps over Bluetooth LE](../requirements/bluetooth.md#req-bluetooth-003-timestamps-over-bluetooth-le)
- [req-engine-001: MIDI I/O on its own thread](../requirements/engine.md#req-engine-001-midi-io-on-its-own-thread)
- [req-engine-002: Input keeps the OS timestamp](../requirements/engine.md#req-engine-002-input-keeps-the-os-timestamp)
- [req-engine-003: Scheduled output on time](../requirements/engine.md#req-engine-003-scheduled-output-on-time)
- [req-engine-004: Cancel until shortly before due](../requirements/engine.md#req-engine-004-cancel-until-shortly-before-due)
- [req-engine-005: Losses are diagnostics](../requirements/engine.md#req-engine-005-losses-are-diagnostics)
- [req-engine-006: Panic silences an output](../requirements/engine.md#req-engine-006-panic-silences-an-output)
- [req-engine-009: One package clock](../requirements/engine.md#req-engine-009-one-package-clock)
- [req-engine-010: Bursts without loss](../requirements/engine.md#req-engine-010-bursts-without-loss)
- [req-network-005: Timestamps over the network](../requirements/network.md#req-network-005-timestamps-over-the-network)
