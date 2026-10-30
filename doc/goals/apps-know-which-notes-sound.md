# Apps know which notes sound

## Status

active since 2026-Q4

## Outcome

An app that merges several inputs, two hands, two devices or a sequencer
and a keyboard, knows at any time which notes sound on which channel
and which value every controller has, and sends a clean stream to its
output: no stuck notes, no double Note On, a Note Off only when the last
finger lifts.

## Measure

- The note matrix and the controller matrix of `aud_midi_state_matrix`
  pass tests for every sending rule of decision state-001
- The matrices sit in front of an output in an example app and no note
  sticks when two inputs play the same note

## Requirements

- [req-state-001: The note matrix counts Note Ons](../requirements/state.md#req-state-001-the-note-matrix-counts-note-ons)
- [req-state-002: Clean Note On and Note Off output](../requirements/state.md#req-state-002-clean-note-on-and-note-off-output)
- [req-state-003: Retrigger per channel](../requirements/state.md#req-state-003-retrigger-per-channel)
- [req-state-004: Release all and reset](../requirements/state.md#req-state-004-release-all-and-reset)
- [req-state-005: The controller matrix keeps the last value](../requirements/state.md#req-state-005-the-controller-matrix-keeps-the-last-value)
- [req-state-006: Chainable between inputs and outputs](../requirements/state.md#req-state-006-chainable-between-inputs-and-outputs)
- [req-state-007: Channel mode messages](../requirements/state.md#req-state-007-channel-mode-messages)
- [req-state-008: MIDI 2.0 notes and controllers](../requirements/state.md#req-state-008-midi-20-notes-and-controllers)
- [req-messages-009: Notes and voices as values](../requirements/messages.md#req-messages-009-notes-and-voices-as-values)
