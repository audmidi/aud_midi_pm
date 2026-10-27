# Changelog

## Unreleased

### Changed

- Plan aud_midi_02: the note and controller state matrices

Plan file of ticket aud_midi_02
Decision state-001: note cells are stacks of velocity slots
Architecture row and package graph entry for aud_midi_state_matrix
Shared types MidiKey and MidiMessageCallback go to aud_midi_standard

## 0.1.1 - 2026-10-24

### Changed

- Make all repos run on Windows

## 0.1.0 - 2026-10-12

### Added

- Add the native taps and port identity of aud_audio ticket 26

- native-001: backends hand input to a native tap on their receive thread

- ports-001: consumers find a re-plugged port by its fingerprint

- A link to the plan of ticket 26 in aud_audio_pm

- Add the Bluetooth results to native-001

- The GO:KEYS over USB and BLE; the app as a BLE peripheral as open work


## 0.0.1 - 2026-10-07

### Added

- Add boilerplate

### Changed

- Upgrade dependencies
- Move the plan to aud_midi_pm
- Rename the PM repo to aud_midi_pm and move the plan there
- Document the aud_midi implementation
- Reference the aud_midi packages on pub.dev and the audmidi organization
- Point the architecture to the audmidi organization
- Plan BLE peripheral support
- Merge main

## 0.0.0 - 2026-10-06

- Initial boilerplate
