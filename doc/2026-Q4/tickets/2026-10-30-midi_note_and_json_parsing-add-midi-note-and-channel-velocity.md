# midi_note_and_json_parsing: Add MidiNote and ChannelVelocity

## Goal

Apps that render notes need a note with its velocity and a voice, a
channel with a velocity, as shared values next to `MidiKey`.
`aud_midi_standard` gets both. The ticket serves the goal
[apps know which notes sound](../../goals/apps-know-which-notes-sound.md).

The ticket first also planned to generate the JSON code of the models
with `json_serializable`. The user dropped that on 2026-10-30: the models
keep their hand-written readers, and `aud_midi_standard` keeps depending
on the Dart SDK only
([req-constraints-010](../../requirements/constraints.md#req-constraints-010-thin-dependencies-of-the-building-blocks)).

## Requirements

- [req-messages-009: Notes and voices as values](../../requirements/messages.md#req-messages-009-notes-and-voices-as-values),
  engineered with the user on 2026-10-30

## Affected repos

| Repo                | Change                                  |
| ------------------- | --------------------------------------- |
| `aud_midi_pm`       | req-messages-009, this plan             |
| `aud_midi_standard` | `MidiNote`, `ChannelVelocity`; patch, 0.3.1 |

## Design

- `MidiNote`: final class with `channel` (0 to 15), `note` and `velocity`
  (0 to 127), asserted like `MidiKey`; `MidiKey get key`, `copyWith`,
  `fromJson` and `toJson` with the keys `channel`, `note` and `velocity`,
  equality by value. It is composed, not derived from `MidiKey` (decided
  2026-10-30): `MidiKey` is a final class and compares `other is MidiKey`,
  so a subclass would compare asymmetrically
- `ChannelVelocity`: final class with `channel` and `velocity`;
  `MidiNote note(int note)` creates the note of a note number; `copyWith`,
  JSON, equality by value
- JSON with `MidiJsonReader`, like `MidiKey`
- Version 0.3.1: the classes are new, nothing changes, so
  `aud_midi_state_matrix`, `aud_midi_core`, `aud_midi`, `aud_midi_network`
  and `aud_midi_rtp` keep `^0.3.0` and need no release

## Steps

1. req-messages-009 and this plan (2026-10-30)
2. `MidiNote` and `ChannelVelocity` with tests
3. README in English and German, `index.jsonc`, publish config (patch)

## Open questions

None.
