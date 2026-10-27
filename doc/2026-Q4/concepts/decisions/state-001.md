# state-001: Note cells are stacks of velocity slots

- Status: accepted
- Date: 2026-10-27
- Canonical source: ticket aud_midi_02, user decision while planning
- Open work: Reset All Controllers (CC 121), source-aware release, MIDI
  2.0 messages

`MidiNoteMatrix` keeps one cell per channel and note with velocity slots,
initially five, doubled when full up to ten, 0 meaning free. A Note On
writes its velocity into the first free slot, a Note Off frees the slot
filled last, so the occupied slots are always contiguous from the front
and their count says how many Note Ons hold the note. A full cell counts
no further Note On; the newest velocity replaces the slot filled last, so
a stream of Note Ons without Note Offs cannot grow the matrix. Towards `midiOut` the matrix
sends the first Note On, nothing for the Note Offs that leave the note
held, and the last Note Off as received. A repeated Note On sends a Note
Off with release velocity 64 followed by the new Note On while
`retriggerRepeatedNoteOns` is true for the channel, and nothing while it
is false. A Note On with velocity 0 counts as Note Off. A Note Off
without a held note is forwarded unchanged, so no note can get stuck
behind the matrix. All Sound Off, All Notes Off and the mode messages
that imply it (CC 120, 123 to 127) send a Note Off for every held note of
their channel and pass through after them; `release`, `releaseChannel`
and `releaseAll` do the same on request, so a Note Off that never
arrives can be made up for by the app. All other messages pass through
unchanged and in order, so `MidiNoteMatrix` and `MidiControllerMatrix`
can sit in a chain in front of an output.
