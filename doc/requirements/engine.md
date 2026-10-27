# Engine

What the runtime of `aud_midi_core` must do: threading, timing,
diagnostics and the backend contract. Accepted with the user on
2026-10-27 in ticket aud_midi_03.

## req-engine-001: MIDI I/O on its own thread

- Status: accepted
- Priority: must
- Statement: All MIDI I/O runs on a thread of its own that no isolate of
  the app can block; on the web, where the browser has MIDI on the main
  thread only, it runs there
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy;
  decision isolate-001
- Acceptance: `midi_bench` with an app isolate that blocks for 100 ms at
  a time still meets req-engine-003

## req-engine-002: Input keeps the OS timestamp

- Status: accepted
- Priority: must
- Statement: Every received message carries the time of its arrival as
  stamped by the operating system or the transport, mapped onto the
  package clock
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: two messages sent 10 ms apart through a loopback arrive
  with timestamps 10 ms apart within 1 ms, while the app isolate sleeps
  100 ms

## req-engine-003: Scheduled output on time

- Status: accepted
- Priority: must
- Statement: A message sent with a due time reaches the port within 1 ms
  of that time at p99 on platforms whose operating system schedules
  MIDI, and within 5 ms at p99 elsewhere, while the app isolate is busy
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy;
  decision scheduling-001
- Acceptance: `midi_bench` with 500 scheduled messages and a busy app
  isolate reports arrival minus due time under 1 ms at p99 on macOS,
  iOS, Android 10 and later, Linux and Windows with Windows MIDI
  Services, and under 5 ms at p99 on Windows without them, on Android
  before 10 and on the web, where background tabs throttle timers

## req-engine-004: Cancel until shortly before due

- Status: accepted
- Priority: should
- Statement: On every platform but the web, a scheduled message can be
  cancelled until 100 ms before its due time, and cancelling an output
  drops every message pending for it; on the web, messages go to the
  browser at once and cannot be cancelled
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy;
  decision scheduling-001
- Acceptance:
  - on macOS, iOS, Android, Windows and Linux a note scheduled 1 s ahead
    and cancelled after 500 ms never arrives
  - after cancelling the output, no scheduled message arrives

## req-engine-005: Losses are diagnostics

- Status: accepted
- Priority: must
- Statement: A loss, an input queue overflow, a dropped SysEx fragment,
  an untranslatable message or a network loss, is reported as a
  diagnostic with a count and never errors a stream
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy
- Acceptance:
  - an input flooded beyond its queue delivers diagnostics whose counts
    add up to the number of dropped messages
  - the stream of that input keeps delivering afterwards

## req-engine-006: Panic silences an output

- Status: accepted
- Priority: must
- Statement: On request an output silences every note on every channel,
  All Notes Off, All Sound Off and sustain off, and drops the messages
  still scheduled for it
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy
- Acceptance: a loopback receiver sees All Notes Off and All Sound Off
  on all 16 channels after a panic and no scheduled note afterwards

## req-engine-007: One contract for every backend

- Status: accepted
- Priority: must
- Statement: Every backend fulfils one contract proven by shared tests
- Why: [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, test without hardware
- Acceptance: the contract tests run against every platform backend and
  against the fake backend

## req-engine-008: Any message to any port

- Status: accepted
- Priority: must
- Statement: An output accepts any message and translates it to the
  protocol of its port, MIDI 1.0 bytes or UMP; a byte port maps to one
  configurable UMP group
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device; decision translation-001
- Acceptance: a MIDI 2.0 Note On sent to a byte port arrives as a MIDI
  1.0 Note On with the velocity scaled as the specification says

## req-engine-009: One package clock

- Status: accepted
- Priority: must
- Statement: All timestamps of received messages and all due times of
  sent messages lie on one monotonic package clock that the app reads at
  any time
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy
- Acceptance: a message received through a loopback right after sending
  has a timestamp between the clock reading before the send and the
  reading after the receive

## req-engine-010: Bursts without loss

- Status: accepted
- Priority: should
- Statement: A burst of 1 000 messages within 100 ms on one input and a
  sustained rate of 2 000 messages per second on one input reach the
  app without a loss
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, play a sequence while the UI is busy
- Acceptance: a loopback test sends 1 000 controller messages in 100 ms
  and 2 000 per second for 10 s; every message arrives and no overflow
  diagnostic is raised

## req-engine-011: Long SysEx

- Status: accepted
- Priority: should
- Statement: A system exclusive message of 1 MB, a firmware or patch
  dump, passes an input and an output complete and in order with the
  messages around it
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: a 1 MB SysEx sent through a loopback arrives as one
  message with identical bytes, and a clock sent during it arrives too

## req-engine-012: Jitter-reduction timestamps

- Status: accepted
- Priority: could
- Statement: Jitter-reduction timestamps of UMP streams are applied to
  received messages and generated for sent messages where the port
  supports them
- Why: [MIDI 2.0 without a rewrite](../goals/midi-2-without-a-rewrite.md),
  topic `scenarios`, use a MIDI 2.0 device
- Acceptance: a UMP input that carries JR timestamps delivers messages
  with the spacing the timestamps encode, not the spacing of their
  arrival

## req-engine-013: A fake backend for tests

- Status: accepted
- Priority: must
- Statement: A fake backend with scripted ports and messages lets apps
  and packages test without hardware
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware
- Acceptance:
  - the example app has a test that runs on the fake backend
  - a test scripts a port that appears, delivers messages and vanishes,
    without hardware

## req-engine-014: Delivery within a millisecond

- Status: accepted
- Priority: must
- Statement: A round trip from the app through a virtual port and back
  to the app completes within 1 ms at p99
- Why: [timing a busy app cannot disturb](../goals/timing-a-busy-app-cannot-disturb.md),
  topic `scenarios`, plug in a USB keyboard
- Acceptance: `midi_bench` reports the round trip through virtual
  endpoints to the app isolate under 1 ms at p99 on every platform with
  virtual ports
