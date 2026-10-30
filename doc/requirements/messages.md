# Messages

What the message layer of `aud_midi_standard` must do: typed messages,
codecs, translation and constants. Accepted with the user on 2026-10-27 in
ticket aud_midi_03.

## req-messages-001: All MIDI 1.0 messages typed

- Status: accepted
- Priority: must
- Statement: Every MIDI 1.0 message, channel voice, system common, system
  real-time and system exclusive, exists as a typed message with its
  fields
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: one test per status byte of the MIDI 1.0 specification
  constructs the message and reads its fields back

## req-messages-002: All UMP messages typed

- Status: accepted
- Priority: must
- Statement: Every UMP message type exists as a typed message with its
  group and fields: utility (MT0), system (MT1), MIDI 1.0 channel voice
  (MT2), SysEx7 (MT3), MIDI 2.0 channel voice (MT4), SysEx8 and mixed
  data set (MT5), flex data (MTD) and UMP stream (MTF)
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device
- Acceptance: every opcode of the UMP 1.1 specification has a message
  and a decode-encode round-trip test

## req-messages-003: Byte stream codec

- Status: accepted
- Priority: must
- Statement: A MIDI 1.0 byte stream is parsed into messages, with running
  status, real-time messages interleaved in other messages and SysEx
  split over packets, and messages are encoded back into bytes
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance:
  - every message survives an encode-parse round trip
  - a SysEx split over three packets with clocks in between is
    reassembled, the clocks are delivered in between

## req-messages-004: UMP codec

- Status: accepted
- Priority: must
- Statement: UMP words are decoded into messages and messages are
  encoded into UMP words, one to four words per packet with their group
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device
- Acceptance:
  - every message type survives an encode-decode round trip
  - a malformed word produces a diagnostic and the decoder goes on with
    the next packet

## req-messages-005: Translation between MIDI 1.0 and MIDI 2.0

- Status: accepted
- Priority: must
- Statement: Messages are translated in both directions by the rules of
  the UMP specification: scaling of values, RPN and NRPN to registered
  and assignable controllers and back, bank select folding; a message
  without an equivalent is reported, never dropped silently
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device; decision translation-001
- Acceptance:
  - one test per rule of translation-001, values checked against the
    conversion tables of the specification
  - a MIDI 2.0 per-note pitch bend sent to a MIDI 1.0 port yields a
    diagnostic, not an exception

## req-messages-006: BLE-MIDI framing

- Status: accepted
- Priority: must
- Statement: BLE-MIDI packets are encoded and decoded with their header
  and timestamp bytes, running status and SysEx split over packets
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, connect a Bluetooth LE keyboard
- Acceptance: the packet examples of the BLE-MIDI 1.0 specification
  decode to the expected messages and timestamps and encode back

## req-messages-007: Constants by name

- Status: accepted
- Priority: must
- Statement: Note numbers, controller numbers, status bytes and the other
  constants of the standards are available by name, as the successor of
  `gg_midi_vars`
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, merge two inputs into one output
- Acceptance: every controller of the MIDI 1.0 controller table and
  every note from 0 to 127 has a named constant

## req-messages-008: Models without the runtime

- Status: accepted
- Priority: must
- Statement: Ports, devices, capabilities, events, diagnostics and
  network sessions exist as plain data models that a program uses
  without the runtime of the family
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware
- Acceptance: a Dart program that depends on `aud_midi_standard` alone
  constructs a port, a device and a diagnostic and compares them by value

## req-messages-009: Notes and voices as values

- Status: accepted
- Priority: must
- Statement: A note, its channel, note number and velocity, and a voice, a
  channel and a velocity, exist as immutable values that compare by value
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, notes that several inputs play; ticket
  midi_note_and_json_parsing
- Acceptance:
  - a Dart program that depends on `aud_midi_standard` alone creates a
    `MidiNote` from a `ChannelVelocity` and a note number, reads its
    `MidiKey` and compares two notes by value
  - `MidiNote` and `ChannelVelocity` survive `toJson` and `fromJson`
    unchanged
