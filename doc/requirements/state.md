# State

What the note matrix and the controller matrix of `aud_midi_state_matrix`
must do. Accepted with the user on 2026-10-27 in ticket aud_midi_03; the
design is in the plan of ticket
[aud_midi_02](../2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md)
and in decision state-001.

## req-state-001: The note matrix counts Note Ons

- Status: accepted
- Priority: must
- Statement: The note matrix keeps, per channel and note, every open Note
  On with its velocity, so an app reads which notes sound and how often
  each one is held
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output
- Acceptance:
  - two Note Ons of C4 on channel 0 give a count of 2 and both
    velocities, and the list of active notes holds C4 once
  - one Note Off brings the count to 1 and C4 stays active

## req-state-002: Clean Note On and Note Off output

- Status: accepted
- Priority: must
- Statement: In front of an output the note matrix sends a Note On only
  when a note starts to sound and a Note Off only when the last Note On
  of that note is released; a Note Off of a silent note passes through,
  and every other message passes through unchanged and in order
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output;
  decision state-001
- Acceptance:
  - every row of the sending table of state-001 is a test
  - a stream of 1 000 random Note Ons and Note Offs from two inputs
    leaves no note sounding at the output once all inputs have released

## req-state-003: Retrigger per channel

- Status: accepted
- Priority: must
- Statement: Per channel the app chooses whether a repeated Note On of a
  sounding note retriggers it, a Note Off and then the new Note On, or is
  only counted; retrigger is the default
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output;
  decision state-001
- Acceptance:
  - with retrigger on, the second Note On yields a Note Off with release
    velocity 64 and then the new Note On
  - with retrigger off, the second Note On yields nothing

## req-state-004: Release all and reset

- Status: accepted
- Priority: must
- Statement: On request the note matrix sends a Note Off for every
  sounding note and clears, or clears silently
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output
- Acceptance:
  - after three notes, release all sends three Note Offs and the list of
    active notes is empty
  - reset sends nothing and the list is empty

## req-state-005: The controller matrix keeps the last value

- Status: accepted
- Priority: must
- Statement: The controller matrix keeps the last value of every
  controller per channel, unset until a value arrives, lists the values
  that are set and resets per channel or as a whole; every message
  passes through unchanged
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output
- Acceptance:
  - two Control Changes of controller 7 leave the second value, and the
    list of entries holds it once
  - resetting the channel makes the value unset again
  - the messages reach the callback unchanged and in order

## req-state-006: Chainable between inputs and outputs

- Status: accepted
- Priority: must
- Statement: Both matrices take messages in and hand them to a callback,
  so they sit anywhere in a chain between inputs and outputs
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output
- Acceptance:
  - the example chains the note matrix, the controller matrix and an
    output
  - every input call that produces messages calls the callback once
    with all of them in order

## req-state-007: Channel mode messages

- Status: accepted
- Priority: could
- Statement: The matrices interpret All Notes Off, All Sound Off and
  Reset All Controllers: the note matrix releases the notes of the
  channel, the controller matrix resets the channel
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  topic `scenarios`, merge two inputs into one output; open point of
  the plan of ticket aud_midi_02
- Acceptance: after All Notes Off on channel 0 the note matrix lists no
  active note on that channel and has sent a Note Off for each one;
  after Reset All Controllers the controller matrix has no value set on
  that channel

## req-state-008: MIDI 2.0 notes and controllers

- Status: accepted
- Priority: could
- Statement: The matrices count MIDI 2.0 Note On and Note Off and keep
  MIDI 2.0 Control Change values like their MIDI 1.0 counterparts
- Why: [apps know which notes sound](../goals/apps-know-which-notes-sound.md),
  [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device; open point of the plan of
  ticket aud_midi_02
- Acceptance: a MIDI 2.0 Note On and a MIDI 1.0 Note On of the same note
  give a count of 2, and only the second Note Off releases the note
