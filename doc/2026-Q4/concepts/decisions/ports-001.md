# ports-001: Consumers find a re-plugged port by its fingerprint

- Status: accepted
- Date: 2026-10-12
- Canonical source: ticket 26 of aud_audio ([plan](https://github.com/audaudio/aud_audio_pm/blob/main/doc/2026-Q4/tickets/2026-10-12-26-bridge-aud-midi-ports-into-the-engine-on-ios-and-android.md), decision 4 of
  its plan review)
- Open work: two identical devices without a serial number stay
  ambiguous; CoreMIDI's id stability checked with USB hardware

Port ids stay as they are and hold for a session: CoreMIDI keeps its
`uniqueID`, and an endpoint that goes offline stays listed, so a
re-plugged device returns under its id; Android gives a re-plugged device
a new id. A consumer that keeps connections across a re-plug, such as the
aud_audio bridge, stores the port's `fingerprint` (manufacturer, name,
serial number, direction, index) next to its id and rebinds to the port
that `Midi.replugCandidates` offers. Deriving stable ids inside
`aud_midi_android` would change the ids for every user of aud_midi.
