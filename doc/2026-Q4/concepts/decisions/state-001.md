# state-001: Note cells are stacks of velocity slots

- Status: accepted
- Date: 2026-10-27
- Canonical source: ticket aud_midi_02, user decision while planning
- Open work: channel mode messages (CC 120, 121, 123), MIDI 2.0 messages

`MidiNoteMatrix` keeps one cell per channel and note with velocity slots,
initially five, doubled when full, 0 meaning free. A Note On writes its
velocity into the first free slot, a Note Off frees the slot filled last,
so the occupied slots are always contiguous from the front and their
count says how many Note Ons hold the note. Towards `midiOut` the matrix
sends the first Note On, nothing for the Note Offs that leave the note
held, and the last Note Off as received. A repeated Note On sends a Note
Off with release velocity 64 followed by the new Note On while
`retriggerRepeatedNoteOns` is true for the channel, and nothing while it
is false. A Note On with velocity 0 counts as Note Off. A Note Off
without a held note is forwarded unchanged, so no note can get stuck
behind the matrix. Messages that are neither Note On nor Note Off pass
through unchanged and in order, so `MidiNoteMatrix` and
`MidiControllerMatrix` can sit in a chain in front of an output.
