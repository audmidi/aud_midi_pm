# Network

What the family must do with MIDI over IP networks: AppleMIDI sessions,
the RTP-MIDI recovery journal, discovery and Network MIDI 2.0. Accepted
with the user on 2026-10-27 in ticket aud_midi_03.

## req-network-001: AppleMIDI sessions

- Status: accepted
- Priority: must
- Statement: The app joins AppleMIDI sessions that other hosts offer and
  offers sessions of its own, interoperable with the macOS and iOS driver
  and with rtpMIDI on Windows
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  [the app is a MIDI device](../goals/the-app-is-a-midi-device.md),
  topic `scenarios`, join a session over Wi-Fi; decision network-001
- Acceptance:
  - a session with Audio MIDI Setup on a Mac exchanges notes in both
    directions from Android, Windows and Linux
  - a session with rtpMIDI on Windows does the same

## req-network-002: Losses repaired by the journal

- Status: accepted
- Priority: must
- Statement: Losses on the network are repaired from the RTP-MIDI
  recovery journal on both sides, so a lost packet never leaves a stuck
  note or a stale controller value
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, join a session over Wi-Fi; decision network-001
- Acceptance:
  - through the lossy UDP proxy with 20 % loss, the note and controller
    state of the receiver equals the state of the sender at the end of a
    stream of 10 000 messages
  - a lost Note Off is recovered with the next packet that arrives

## req-network-003: Discovery and advertising

- Status: accepted
- Priority: must
- Statement: The app lists the sessions advertised on the local network
  and advertises its own session by DNS-SD on every platform that has
  network sessions
- Why: [the app is a MIDI device](../goals/the-app-is-a-midi-device.md),
  topic `scenarios`, join a session over Wi-Fi
- Acceptance:
  - a session advertised by a Mac appears within 5 s on Android, Windows
    and Linux
  - the own session of the app appears in Audio MIDI Setup on a Mac in
    the same network

## req-network-004: Network MIDI 2.0

- Status: accepted
- Priority: should
- Statement: The app joins and offers Network MIDI 2.0 sessions, UMP over
  UDP, with the authentication the host requires
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, join a session over Wi-Fi; decision network-001
- Acceptance:
  - two instances exchange UMP over localhost with shared-secret and
    with user-and-password authentication
  - a wrong secret is rejected and reported

## req-network-005: Timestamps over the network

- Status: accepted
- Priority: should
- Statement: Messages received over a session carry the timestamps of the
  sender mapped onto the local package clock after clock synchronisation
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, join a session over Wi-Fi
- Acceptance: after the clock synchronisation, two messages sent 10 ms
  apart arrive with timestamps 10 ms apart within 2 ms over the lossless
  proxy

## req-network-006: RTP-MIDI as a codec of its own

- Status: accepted
- Priority: should
- Statement: The RTP-MIDI payload format and the recovery journal are
  usable as a codec without sockets or sessions
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware
- Acceptance: `aud_midi_rtp` encodes and decodes payloads with journals
  in a test without a socket and depends on `aud_midi_standard` only
