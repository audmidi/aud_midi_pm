# aud_midi_pm

Projektmanagement-Repository der Audanika-MIDI-Packages: Ziele, Anforderungen, Pläne, Entscheidungen und Architektur. Kein Code.

Aktuelles Quartal: [2026-Q4](doc/2026-Q4).

## Ziele

Alle Ziele sind seit 2026-Q4 aktiv.

- [Eine MIDI-API für jede Plattform](doc/goals/one-midi-api-for-every-platform.md)
- [Timing, das eine beschäftigte App nicht stört](doc/goals/timing-a-busy-app-cannot-disturb.md)
- [MIDI 2.0 ohne Neuschreiben](doc/goals/midi-2-without-a-rewrite.md)
- [Bausteine für andere MIDI-Software](doc/goals/building-blocks-for-other-midi-software.md)
- [Die App ist ein MIDI-Gerät](doc/goals/the-app-is-a-midi-device.md)
- [Apps wissen, welche Noten klingen](doc/goals/apps-know-which-notes-sound.md)
- [Auf echten Geräten verifiziert](doc/goals/verified-on-real-devices.md)

## Anforderungen

Eine Datei pro Bereich, IDs `req-<area>-<nnn>`.

- [API](doc/requirements/api.md): die App-seitige API von `aud_midi`
- [Messages](doc/requirements/messages.md): typisierte Nachrichten, Codecs, Übersetzung und Konstanten von `aud_midi_standard`
- [Engine](doc/requirements/engine.md): Threading, Timing, Diagnostik und der Backend-Vertrag von `aud_midi_core`
- [Ports](doc/requirements/ports.md): Hotplug, Identität, virtuelle Ports und Fähigkeiten
- [Network](doc/requirements/network.md): AppleMIDI-Sessions, das RTP-MIDI-Journal, Discovery und Network MIDI 2.0
- [Bluetooth](doc/requirements/bluetooth.md): die App als Bluetooth-LE-Central und -Peripheral
- [Platforms](doc/requirements/platforms.md): was jedes Betriebssystem bietet und ab welcher Version
- [State](doc/requirements/state.md): die Noten-Matrix und die Controller-Matrix von `aud_midi_state_matrix`
- [Constraints](doc/requirements/constraints.md): feste Technologien, Package-Regeln und Qualitätsschranken

## Quartal 2026-Q4

- [Tickets](doc/2026-Q4/tickets)
- [Entscheidungen](doc/2026-Q4/concepts/decisions/000-index.md)
- [Architektur](doc/2026-Q4/architecture/architecture.md)
- [Szenarien: die Menschen und die Hauptszenarien der Familie](doc/2026-Q4/concepts/topics/scenarios.md)
- [Plan aud_midi_01: Initiale MIDI-Implementierung](doc/2026-Q4/tickets/2026-10-06-aud_midi_01-initial-midi-implementation.md)
- [Referenzen: Spezifikationen und Referenzimplementierungen](doc/references/README.md)
