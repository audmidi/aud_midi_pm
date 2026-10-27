# aud_midi_02: Track active notes

## Goal

Give apps a place that knows which MIDI notes sound right now and which
controller values are current. The new package `aud_midi_state_matrix`
holds both as matrices of 16 channels × 128 notes or controllers. The
note matrix keeps every Note On of a note in a velocity slot, so several
Note Ons of the same note (two fingers, two merged inputs) are counted
and only the last Note Off releases the note towards the output. The
controller matrix keeps the last value of every controller per channel.
Both sit in a chain in front of a MIDI output through a `midiOut`
callback.

## Requirements

Engineered in ticket aud_midi_03 and kept in
[requirements/state.md](../../requirements/state.md). This ticket fulfils
req-state-001 to req-state-006 and the note matrix part of req-state-007
(All Sound Off, All Notes Off and the mode messages that imply it). Still
open: Reset All Controllers in the controller matrix (req-state-007) and
MIDI 2.0 notes and controllers (req-state-008).

## Affected repos

| Repo | Change |
| --- | --- |
| `aud_midi_pm` | this plan, decision state-001, a row in the architecture and the package graph |
| `aud_midi_standard` | the shared types `MidiKey` (channel and note of a note message) and `MidiMessageCallback` (receiver of message lists) |
| `aud_midi_state_matrix` | new repo: `MidiNoteMatrix`, `MidiControllerMatrix`, example, READMEs, `index.jsonc` |

The new package is pure Dart and depends on `aud_midi_standard` only
(`MidiMessage`, `MidiNoteOn`, `MidiNoteOff`, `MidiControlChange`,
`MidiKey`, `MidiMessageCallback`). Shared vocabulary lives in
`aud_midi_standard`, not in a repo of its own.

## Design

### MidiNoteMatrix

- 16 channels × 128 notes. Every cell holds velocity slots, initially
  five (`initialSlots`), value 0 means free. When all slots of a cell are
  taken, the cell doubles its slots, up to `maxSlots` (default 10). A
  full cell counts no further Note On: the newest velocity replaces the
  slot filled last, so a stream of Note Ons without Note Offs cannot
  grow the matrix.
- A cell is a stack: a Note On writes its velocity into the first free
  slot, a Note Off frees the slot filled last. Occupied slots are always
  contiguous from the front, the first free slot is always `count`
  (state-001).
- `midiIn(List<MidiMessage>)` handles Note On and Note Off; a Note On
  with velocity 0 counts as Note Off. Every other message passes through
  unchanged and in order, so the matrix can sit in a chain.
- `midiOut` is a `MidiMessageCallback` of `aud_midi_standard`,
  `void Function(List<MidiMessage>)`. Every `midiIn` call that produces
  messages calls it once with all of them in order.
- `retriggerRepeatedNoteOns(channel)` reads the per-channel flag,
  `setRetriggerRepeatedNoteOns(channel:, value:)` sets it, default
  `true`.
- `activeNotes` is a maintained list of `MidiKey(channel, note)` of
  `aud_midi_standard`: an entry joins when a cell goes from 0 to 1 and
  leaves when it goes from 1 to 0. `velocities`, `slots`, `count` and
  `isActive` read a cell.
- `reset()` clears silently; `release(channel:, note:)`,
  `releaseChannel(channel)` and `releaseAll()` send a Note Off for every
  held note in their scope and free the cells, so the app can end notes
  whose Note Off will never arrive.
- All Sound Off (CC 120), All Notes Off (CC 123) and the mode messages
  that imply All Notes Off (CC 124 to 127) release the held notes of
  their channel the same way and pass through after the Note Offs.

The sending rules per cell, `n` is the number of occupied slots before
the message:

| Input | State | Retrigger on | Retrigger off |
| --- | --- | --- | --- |
| Note On | n = 0 | send the Note On | send the Note On |
| Note On | n ≥ 1 | Note Off (release 64), then the new Note On | nothing |
| Note Off | n ≥ 2 | nothing | nothing |
| Note Off | n = 1 | send the Note Off as received | same |
| Note Off | n = 0 | forward unchanged (no stuck notes) | same |

### MidiControllerMatrix

- 16 channels × 128 controllers, per cell the last value 0 to 127,
  `null` until a value was set.
- Same shape as the note matrix: `midiIn` records every Control Change
  and passes all messages through unchanged. `value`, `isSet`, `set`,
  `reset`, `resetChannel` and `entries` (a list of `MidiControlChange`,
  ready to be sent) read and change the state.
- Not in this ticket: 14-bit pairs (MSB/LSB), RPN/NRPN and a special
  handling of Reset All Controllers (CC 121).

## Steps

1. Add `aud_midi_pm` to the ticket, write this plan and state-001.
2. Create `aud_midi_state_matrix` with `gg do create repo`, add the
   dependency on `aud_midi_standard`.
3. Add `MidiKey` and `MidiMessageCallback` to `aud_midi_standard` with
   tests; implement `MidiNoteMatrix` with tests.
4. Implement `MidiControllerMatrix` with tests; its `entries` are
   `MidiControlChange`.
5. Example, README in English and German, `index.jsonc`, publish config.
6. `gg can commit` green with 100 % coverage, one commit per repo; push,
   review and publish on the user's sign.

## Open questions

Decided with the user on 2026-10-27 and recorded in
[state-001](../concepts/decisions/state-001.md): a Note Off frees the
slot filled last; an unmatched Note Off is forwarded; foreign messages
pass through; the input is called `midiIn`; the controller matrix is
implemented in this ticket. Also decided on 2026-10-27: the shared types
go to `aud_midi_standard` as `MidiKey` and `MidiMessageCallback` instead
of a repo of their own, and the entries of the controller matrix are
`MidiControlChange` messages.

Still open, for later tickets:

- Reset All Controllers (CC 121) is stored like any value; the
  controller matrix does not interpret it.
- Stuck notes beyond the channel mode messages and the release hooks:
  remember the source of every held note (`midiIn(messages, source:)`,
  `releaseSource(source)`) so that a vanished port or touch session
  releases only its own notes, and an optional time limit driven by the
  app (`sweep(now)`, `maxHold` per channel).
- MIDI 2.0 channel voice messages (`MidiNoteOn2`, `MidiControlChange2`,
  …) are passed through; translate them with `toMidi1()` first.
