# Bluetooth

What the family must do with Bluetooth LE MIDI: the app as a central, the
app as a peripheral, timestamps and reconnects. Accepted with the user on
2026-10-27 in ticket aud_midi_03.

## req-bluetooth-001: The app as a Bluetooth LE central

- Status: accepted
- Priority: must
- Statement: The app scans for Bluetooth LE MIDI devices, connects to
  them and uses them as ports on iOS, macOS, Android and Linux; on
  Windows a device paired from the app or in the operating system
  appears as a port
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, connect a Bluetooth LE keyboard
- Acceptance:
  - a BLE-MIDI keyboard connects and plays on iOS, macOS, Android and
    Linux
  - on Windows a keyboard paired in the operating system appears as a
    port and plays

## req-bluetooth-002: The app as a Bluetooth LE peripheral

- Status: accepted
- Priority: should
- Statement: The app advertises itself as a Bluetooth LE MIDI device, so
  that hosts connect to it, on iOS, macOS, Android, Windows and Linux
- Why: [the app is a MIDI device](../goals/the-app-is-a-midi-device.md),
  topic `scenarios`, connect a Bluetooth LE keyboard
- Acceptance: an iPad connects to the app on each of the five platforms
  and exchanges notes in both directions

## req-bluetooth-003: Timestamps over Bluetooth LE

- Status: accepted
- Priority: should
- Statement: Messages received over Bluetooth LE carry the timestamps of
  their BLE-MIDI packets mapped onto the package clock, and sent packets
  carry timestamps
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, connect a Bluetooth LE keyboard
- Acceptance: two notes 10 ms apart in their BLE-MIDI timestamps arrive
  with timestamps 10 ms apart within 1 ms

## req-bluetooth-004: Reconnect after a dropout

- Status: accepted
- Priority: could
- Statement: A Bluetooth LE device that drops out of range vanishes as a
  port and returns as the same port when it is back in range
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, connect a Bluetooth LE keyboard; decision ports-001
- Acceptance: switching a keyboard off and on again removes its port and
  restores it with the same fingerprint within 10 s
