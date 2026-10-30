<!--
@license
Copyright (c) audanika

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Plan MidiNote and ChannelVelocity

## Motivation

- The goal "apps know which notes sound" needs a note with its velocity
  and a voice, a channel with a velocity, as shared values. `MidiKey`
  covers the channel and the note number only.
- No accepted requirement covered such values, so the ticket
  `midi_note_and_json_parsing` started with requirements engineering, as
  the PM repo guide asks.
- The ticket first also meant to generate the JSON code of the models
  with `json_serializable`. That collided with a `must` requirement and
  was dropped; this post records both decisions.

## Strategy

- Engineer the requirements with the user, one question at a time, and
  write only what was confirmed.
- Plan the ticket on the accepted requirement, with the affected repos,
  the design decisions and the steps.
- Keep the plan alive: when the scope shrank, the plan and the
  requirements followed in the same ticket.

## Implementation 1: requirement req-messages-009

- New in `doc/requirements/messages.md`: "Notes and voices as values",
  priority `must`. A note, its channel, note number and velocity, and a
  voice, a channel and a velocity, exist as immutable values that compare
  by value.
- Acceptance: a program that depends on `aud_midi_standard` alone builds
  a `MidiNote` from a `ChannelVelocity` and a note number, reads its
  `MidiKey` and compares two notes by value; both survive `toJson` and
  `fromJson`.
- The goal "apps know which notes sound" links the requirement.

## Implementation 2: the plan

- `doc/2026-Q4/tickets/2026-10-30-midi_note_and_json_parsing-add-midi-note-and-channel-velocity.md`,
  in English and German, linked from the tickets README.
- `MidiNote` is composed, not derived from `MidiKey`: `MidiKey` is a final
  class and compares `other is MidiKey`, so a subclass would compare
  asymmetrically.
- `ChannelVelocity.note(int)` creates the note of a note number.
- Release 0.3.1 of `aud_midi_standard`: nothing existing changes, so the
  packages on `^0.3.0` need no release.

## Implementation 3: json_serializable dropped

- Generated JSON code needs `json_annotation` at runtime. That breaks
  req-constraints-010, which keeps `aud_midi_standard` on the Dart SDK
  alone.
- A changed req-constraints-010 and a new requirement for compatible JSON
  were written, then withdrawn on the same day when the user dropped the
  JSON part. Both requirements stand as they were before the ticket.
- The ticket ID still carries "json_parsing"; the plan explains why.

## Dependency updates

- Development tooling only: the DNA layer and the analyzer moved to their
  newest patch and minor versions.

## Open Points

- Set req-messages-009 to `done` once `aud_midi_standard` 0.3.1 is
  published.
