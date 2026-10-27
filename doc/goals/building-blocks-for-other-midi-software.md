# Building blocks for other MIDI software

## Status

active since 2026-Q4

## Outcome

The MIDI standards exist as Dart packages that other projects use on
their own, without the runtime: an audio engine, a sequencer, a test
tool or a converter picks `aud_midi_standard` or `aud_midi_rtp` and gets
messages, codecs, translation and the RTP-MIDI journal without native
code or an isolate.

## Measure

- `aud_midi_standard` depends on the Dart SDK only, `aud_midi_rtp` on
  `aud_midi_standard` alone
- Every package of the family is usable and tested without the umbrella
  `aud_midi`
- At least one project outside the family uses a package of the family
  (the `aud_audio` engine bridges the ports, ticket 26)

## Requirements

- [req-constraints-002: Dart SDK, not Flutter](../requirements/constraints.md#req-constraints-002-dart-sdk-not-flutter)
- [req-constraints-003: One package per scope](../requirements/constraints.md#req-constraints-003-one-package-per-scope)
- [req-constraints-005: README in two languages](../requirements/constraints.md#req-constraints-005-readme-in-two-languages)
- [req-constraints-007: Usable in closed-source apps](../requirements/constraints.md#req-constraints-007-usable-in-closed-source-apps)
- [req-constraints-008: Semantic versions and a changelog](../requirements/constraints.md#req-constraints-008-semantic-versions-and-a-changelog)
- [req-constraints-009: Published on pub.dev](../requirements/constraints.md#req-constraints-009-published-on-pubdev)
- [req-constraints-010: Thin dependencies of the building blocks](../requirements/constraints.md#req-constraints-010-thin-dependencies-of-the-building-blocks)
- [req-engine-013: A fake backend for tests](../requirements/engine.md#req-engine-013-a-fake-backend-for-tests)
- [req-messages-001: All MIDI 1.0 messages typed](../requirements/messages.md#req-messages-001-all-midi-10-messages-typed)
- [req-messages-003: Byte stream codec](../requirements/messages.md#req-messages-003-byte-stream-codec)
- [req-messages-007: Constants by name](../requirements/messages.md#req-messages-007-constants-by-name)
- [req-messages-008: Models without the runtime](../requirements/messages.md#req-messages-008-models-without-the-runtime)
- [req-network-006: RTP-MIDI as a codec of its own](../requirements/network.md#req-network-006-rtp-midi-as-a-codec-of-its-own)
