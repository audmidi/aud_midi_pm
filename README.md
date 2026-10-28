# aud_midi_pm

English | [Deutsch](README.de.md)

Project management repo of the Audanika MIDI packages: plans, decisions,
blog posts. It also holds the goals, requirements and the architecture of
the aud_midi family. No code.

Current quarter: [2026-Q4](doc/2026-Q4).

## Motivation

The aud_midi family spans ten packages, one per scope: the standards, the
engine, one backend per platform, the network sessions and the umbrella.
What they have in common — why they exist, what they must do, which
decisions bind all of them and how the next quarter is planned — belongs
to none of the code repos. This repo keeps it in one place, so every
ticket of the family refers to one plan and one set of decisions.

The project pursues these goals, all active since 2026-Q4:

- [One MIDI API for every platform](doc/goals/one-midi-api-for-every-platform.md)
- [Timing a busy app cannot disturb](doc/goals/timing-a-busy-app-cannot-disturb.md)
- [MIDI 2.0 without a rewrite](doc/goals/midi-2-without-a-rewrite.md)
- [Building blocks for other MIDI software](doc/goals/building-blocks-for-other-midi-software.md)
- [The app is a MIDI device](doc/goals/the-app-is-a-midi-device.md)
- [Apps know which notes sound](doc/goals/apps-know-which-notes-sound.md)
- [Verified on real devices](doc/goals/verified-on-real-devices.md)

## Installation

The repo is not published to pub.dev. Check it out, or add it to a gg
ticket before any code repo:

```bash
git clone https://github.com/audmidi/aud_midi_pm.git
```

```bash
gg do add aud_midi_pm
```

## Usage

Planning runs in this repo, ticket by ticket, with the `gg` workflow and
the planning skills; the
[project management repo guide](doc/guides/pm-repo-guide.md) describes
the structure and the rules:

- `/gg-plan-intro` lists the planning skills and says which one comes next
- `/gg-plan-requirements` engineers the goals and requirements with the
  user, before the first ticket
- `/gg-plan-schedule` cuts the accepted requirements into tickets and
  writes the time plan
- `/gg-plan-jira-export` exports the next tickets of a person as a CSV
  file for Jira
- `/gg-plan-report` keeps the quarterly report of each person up to date

Every ticket of the family gets one plan file in
`doc/<yyyy>-Q<n>/tickets/` and, once finished, one blog post in
`doc/blog/<yyyy>/`. Requirements live in one file per area with IDs
`req-<area>-<nnn>`:

- [API](doc/requirements/api.md): the app-facing API of `aud_midi`
- [Messages](doc/requirements/messages.md): typed messages, codecs,
  translation and constants of `aud_midi_standard`
- [Engine](doc/requirements/engine.md): threading, timing, diagnostics and
  the backend contract of `aud_midi_core`
- [Ports](doc/requirements/ports.md): hotplug, identity, virtual ports and
  capabilities
- [Network](doc/requirements/network.md): AppleMIDI sessions, the RTP-MIDI
  journal, discovery and Network MIDI 2.0
- [Bluetooth](doc/requirements/bluetooth.md): the app as a Bluetooth LE
  central and peripheral
- [Platforms](doc/requirements/platforms.md): what each operating system
  offers and from which version on
- [State](doc/requirements/state.md): the note matrix and the controller
  matrix of `aud_midi_state_matrix`
- [Constraints](doc/requirements/constraints.md): fixed technologies,
  package rules and quality gates

## Documentation

Quarter 2026-Q4:

- [Tickets](doc/2026-Q4/tickets)
- [Decisions](doc/2026-Q4/concepts/decisions/000-index.md)
- [Architecture](doc/2026-Q4/architecture/architecture.md)
- [Scenarios: the people and the main scenarios of the family](doc/2026-Q4/concepts/topics/scenarios.md)
- [Plan aud_midi_01: initial MIDI implementation](doc/2026-Q4/tickets/2026-10-06-aud_midi_01-initial-midi-implementation.md)
- [Plan aud_midi_02: track active notes](doc/2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md)

Across quarters:

- [References: specifications and reference implementations](doc/references/README.md)
- [Blog](doc/blog/)
- [Guides](doc/guides/)

## Contributing

See [doc/guides/develop-guide.md](doc/guides/develop-guide.md) and the
[project management repo guide](doc/guides/pm-repo-guide.md). The repo
keeps two languages: English under `doc/`, German under `doc/de/`, see
[doc/guides/translate-guide.md](doc/guides/translate-guide.md).

## License

MIT — see [LICENSE](LICENSE). The files under `doc/references/` keep
their own licenses, listed in
[doc/references/README.md](doc/references/README.md).
