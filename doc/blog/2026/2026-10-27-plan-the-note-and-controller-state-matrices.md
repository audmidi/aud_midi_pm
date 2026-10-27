# Plan the note and controller state matrices

## Motivation

- The goal "apps know which notes sound" needs a place in the family that
  keeps the state a MIDI stream leaves behind: the sounding notes with
  their velocities and the current controller values.
- Ticket aud_midi_02 plans that place as a package of its own and records
  the decisions on the way, so later tickets and the requirements can
  build on them.

## Strategy

- Plan the ticket in this repo first: goal, affected repos, design, steps
  and open questions in one plan file.
- Record the slot semantics of the note matrix as decision state-001 and
  the package in the architecture and the package graph.
- Decide the open questions with the user before implementing and move
  the answers into the plan and the decision.
- Keep the plan alive while the implementation deviates from it.

## Implementation 1: the plan

- `doc/2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md`
  describes the two matrices, the sending rules per cell as a table and
  the steps of the ticket.
- The affected repos: `aud_midi_pm` for the plan, `aud_midi_standard` for
  the shared types and the new `aud_midi_state_matrix` for the matrices.
- The requirements section links `requirements/state.md`, engineered in
  ticket aud_midi_03; the ticket fulfils req-state-001 to req-state-006
  and the note matrix part of req-state-007, which are set to done.

## Implementation 2: decision state-001

- Note cells are stacks of velocity slots: a Note On fills the first free
  slot, a Note Off frees the slot filled last, so the count says how many
  Note Ons hold the note.
- Only the first Note On and the last Note Off pass to the output; a
  repeated Note On retriggers by the setting of its channel; an unmatched
  Note Off and every foreign message pass through.
- Shared vocabulary goes to `aud_midi_standard` as `MidiKey` and
  `MidiMessageCallback` instead of a repo of its own; the entries of the
  controller matrix are `MidiControlChange` messages.

## Implementation 3: changes during the ticket

- A cell counts at most `maxSlots` (10) Note Ons, so endless Note Ons
  cannot grow the matrix.
- Release paths for stuck notes: `release`, `releaseChannel`,
  `releaseAll` and the channel mode messages that end notes.
- The architecture lists eleven packages now; the package graph shows
  `aud_midi_state_matrix` on `aud_midi_standard`.
- The tickets README and the repo README link the plan.

## Dependency updates

- `dna_audanika` 0.1.0: blog and translate guides, a prompt hook and a
  README check script; the DNA asks for the quick check pipeline name,
  `Quick Check`.

## Open Points

- A follow-up ticket for source-aware release of stuck notes and an
  optional time limit, noted in the plan.
- German copies of the goals, requirements and older plans; this ticket
  translates only the files it added.
