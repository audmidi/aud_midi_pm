# native-001: Backends hand input to a native tap on their receive thread

- Status: accepted
- Date: 2026-10-12
- Canonical source: ticket 26 of aud_audio, the MIDI bridge into the
  audio engine ([plan](https://github.com/audaudio/aud_audio_pm/blob/main/doc/2026-Q4/tickets/2026-10-12-26-bridge-aud-midi-ports-into-the-engine-on-ios-and-android.md), scope 4, decision 1 of its plan review)
- Open work: the native taps of the Linux, Windows and network backends;
  a native send path for outputs; the app as a Bluetooth LE MIDI
  peripheral; checked with a Roland GO:KEYS over USB and BLE on iOS and
  Android (ticket 26); on iOS BLE pairing runs through Apple's
  CABTMIDICentralViewController (MidiBluetooth.showSystemDevicePicker)

A backend that implements `MidiNativeTapBackend` next to `MidiBackend`
calls a native function for an input port on its own receive thread,
before any Dart code runs: `AudMidiTapFunction(user, kind, data, count,
host_time_ns)` with the UMP words of a packet (CoreMIDI), the bytes of a
MIDI 1.0 chunk (Android) or `GONE` when the port disappears or goes
offline. The timestamp is the platform's monotonic host clock in
nanoseconds (`mach_absolute_time` in ns on Apple, `System.nanoTime` on
Android), never 0. Data calls of a tap come from one thread at a time;
GONE may come from the notification thread and is never dropped. One tap
per port; it keeps the port connected without Dart listeners and stays
attached while the port goes offline and returns under the same id. A
detach completes only after the last call returned. `MidiInput` attaches
a tap through the MIDI isolate. The Apple shim calls the tap from slots
with a count of calls in flight and a busy flag instead of a lock; the
Android receiver calls it through JNI from a small C library built by a
build hook. Consumers such as aud_audio's `aud_audio_midi` push into the
audio engine without a hop through Dart.
