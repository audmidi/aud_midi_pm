# Constraints

Fixed technologies, package rules, quality gates and the other
constraints the family works under. Accepted with the user on 2026-10-27
in ticket aud_midi_03.

## req-constraints-001: No platform channels

- Status: accepted
- Priority: must
- Statement: The packages reach the operating system through FFI, JNI
  and JS interop only, never through Flutter platform channels
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, plug in a USB keyboard; plan aud_midi_01,
  decisions 1 and 2
- Acceptance: no package of the family imports the Flutter services
  library or declares a method channel

## req-constraints-002: Dart SDK, not Flutter

- Status: accepted
- Priority: must
- Statement: The packages depend on the Dart SDK, not on Flutter, except
  where a platform forces it: `aud_midi_android` needs the Flutter SDK
  for `jni`, and so does `aud_midi` through it
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware; decision packaging-001
- Acceptance: the tests of every package except `aud_midi_android` and
  `aud_midi` run with the plain Dart SDK

## req-constraints-003: One package per scope

- Status: accepted
- Priority: must
- Statement: The family is split into packages of one scope each, one
  repository per package with the prefix `aud_midi_` in the `audmidi`
  organisation
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware; plan aud_midi_01,
  decision 7
- Acceptance: every repository has an `index.jsonc` with a scope of its
  own, and no two of them claim the same scope

## req-constraints-004: Checks green on three operating systems

- Status: accepted
- Priority: must
- Statement: Every package passes its checks, analysis, formatting and
  tests with 100 % line coverage, on macOS, Linux and Windows before a
  change lands
- Why: [verified on real devices](../goals/verified-on-real-devices.md),
  topic `scenarios`, test without hardware
- Acceptance:
  - `gg can commit` is green on all three operating systems
  - the CI quick check runs on every pull request

## req-constraints-005: README in two languages

- Status: accepted
- Priority: must
- Statement: Every package carries a README in English and in German
  with the same content
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, people
- Acceptance: `README.md` and `README.de.md` exist in every repository
  and change in the same commit

## req-constraints-006: No server, no telemetry

- Status: accepted
- Priority: must
- Statement: The packages contact no server and send data only to the
  MIDI peers the app chose and to the multicast DNS group of the local
  network
- Why: [one MIDI API for every platform](../goals/one-midi-api-for-every-platform.md),
  topic `scenarios`, people
- Acceptance: a network capture during a session shows MIDI, session and
  DNS-SD traffic to the chosen peers and the local network only

## req-constraints-007: Usable in closed-source apps

- Status: accepted
- Priority: should
- Statement: The packages and their dependencies ship under licences
  that allow their use in closed-source apps
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, people
- Acceptance: a licence audit of every dependency in every pubspec finds
  no licence that requires the app to publish its source

## req-constraints-008: Semantic versions and a changelog

- Status: accepted
- Priority: should
- Statement: Every package is versioned semantically, lists its changes
  in a changelog and is installable from a version tag of its
  repository
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, people
- Acceptance: every repository has version tags and a `CHANGELOG.md`
  whose newest entry matches the newest tag

## req-constraints-009: Published on pub.dev

- Status: accepted
- Priority: could
- Statement: The packages are published on pub.dev under their names
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, people
- Acceptance: `dart pub add aud_midi` resolves the newest version

## req-constraints-010: Thin dependencies of the building blocks

- Status: accepted
- Priority: must
- Statement: `aud_midi_standard` depends on the Dart SDK only, and
  `aud_midi_rtp` and `aud_midi_state_matrix` depend on
  `aud_midi_standard` alone
- Why: [building blocks for other MIDI software](../goals/building-blocks-for-other-midi-software.md),
  topic `scenarios`, test without hardware; plan aud_midi_01,
  decision 7
- Acceptance:
  - the pubspec of `aud_midi_standard` lists no dependency
  - the pubspecs of `aud_midi_rtp` and `aud_midi_state_matrix` list
    `aud_midi_standard` as their only dependency
