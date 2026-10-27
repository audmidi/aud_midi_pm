# Tickets

One plan file per ticket, e.g. 2026-10-06-aud_midi_01-initial-midi-implementation.md. The plan of ticket aud_midi_01 is [2026-10-06-aud_midi_01-initial-midi-implementation.md](2026-10-06-aud_midi_01-initial-midi-implementation.md).

The implementation ticket is [audmidi](2026-10-06-audmidi-implement-aud-midi.md).

Ticket aud_midi_02 adds `aud_midi_state_matrix`, the matrices of the
sounding notes and the current controller values:
[2026-10-27-aud_midi_02-track-active-notes.md](2026-10-27-aud_midi_02-track-active-notes.md).

Ticket 26 of aud_audio bridges the aud_midi ports into the audio engine; it
adds the native taps (native-001) and the rule for re-plugged ports
(ports-001). Its plan lives in aud_audio_pm: [plan of ticket 26](https://github.com/audaudio/aud_audio_pm/blob/main/doc/2026-Q4/tickets/2026-10-12-26-bridge-aud-midi-ports-into-the-engine-on-ios-and-android.md).
