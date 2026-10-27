# MIDI 2.0 without a rewrite

## Status

active since 2026-Q4

## Outcome

Apps written against the API today use MIDI 2.0 devices and UMP
endpoints as they appear: through the native UMP path where the
operating system has one and through explicit translation to MIDI 1.0
devices everywhere else. The message model covers the whole UMP
standard, so no MIDI 2.0 feature ever forces an API break.

## Measure

- Every UMP message type (MT0-MT5, MTD, MTF) has a typed message and a
  codec
- The MIDI 1.0 <-> MIDI 2.0 translation follows the rules of the UMP
  specification, proven by tests
- Native UMP ports work on macOS and iOS, Android 13+, Windows MIDI
  Services and ALSA UMP

## Requirements

- [req-engine-008: Any message to any port](../requirements/engine.md#req-engine-008-any-message-to-any-port)
- [req-engine-012: Jitter-reduction timestamps](../requirements/engine.md#req-engine-012-jitter-reduction-timestamps)
- [req-messages-002: All UMP messages typed](../requirements/messages.md#req-messages-002-all-ump-messages-typed)
- [req-messages-004: UMP codec](../requirements/messages.md#req-messages-004-ump-codec)
- [req-messages-005: Translation between MIDI 1.0 and MIDI 2.0](../requirements/messages.md#req-messages-005-translation-between-midi-10-and-midi-20)
- [req-network-004: Network MIDI 2.0](../requirements/network.md#req-network-004-network-midi-20)
- [req-state-008: MIDI 2.0 notes and controllers](../requirements/state.md#req-state-008-midi-20-notes-and-controllers)
