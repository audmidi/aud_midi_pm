# aud_midi_pm

[English](README.md) | Deutsch

Projektmanagement-Repository der Audanika-MIDI-Packages: Pläne,
Entscheidungen, Blogbeiträge. Es hält außerdem die Ziele, Anforderungen
und die Architektur der aud_midi-Familie. Kein Code.

Aktuelles Quartal: [2026-Q4](doc/2026-Q4).

## Motivation

Die aud_midi-Familie umfasst zehn Packages, eines je Scope: die
Standards, die Engine, ein Backend pro Plattform, die Netzwerk-Sessions
und das Dach-Package. Was sie gemeinsam haben — warum es sie gibt, was sie
leisten müssen, welche Entscheidungen für alle gelten und wie das nächste
Quartal geplant ist — gehört in keines der Code-Repos. Dieses Repo hält
es an einem Ort, damit jedes Ticket der Familie auf einen Plan und einen
Satz Entscheidungen verweist.

Das Projekt verfolgt diese Ziele, alle aktiv seit 2026-Q4:

- [Eine MIDI-API für jede Plattform](doc/goals/one-midi-api-for-every-platform.md)
- [Timing, das eine beschäftigte App nicht stört](doc/goals/timing-a-busy-app-cannot-disturb.md)
- [MIDI 2.0 ohne Neuschreiben](doc/goals/midi-2-without-a-rewrite.md)
- [Bausteine für andere MIDI-Software](doc/goals/building-blocks-for-other-midi-software.md)
- [Die App ist ein MIDI-Gerät](doc/goals/the-app-is-a-midi-device.md)
- [Apps wissen, welche Noten klingen](doc/goals/apps-know-which-notes-sound.md)
- [Auf echten Geräten verifiziert](doc/goals/verified-on-real-devices.md)

## Installation

Das Repo wird nicht auf pub.dev veröffentlicht. Auschecken oder vor jedem
Code-Repo zu einem gg-Ticket hinzufügen:

```bash
git clone https://github.com/audmidi/aud_midi_pm.git
```

```bash
gg do add aud_midi_pm
```

## Verwendung

Die Planung läuft in diesem Repo, Ticket für Ticket, mit dem
`gg`-Workflow und den Planungs-Skills; der
[Guide zum Projektmanagement-Repo](doc/guides/pm-repo-guide.md)
beschreibt die Struktur und die Regeln:

- `/gg-plan-intro` listet die Planungs-Skills und sagt, welcher als
  Nächstes kommt
- `/gg-plan-requirements` erarbeitet die Ziele und Anforderungen mit dem
  Nutzer, vor dem ersten Ticket
- `/gg-plan-schedule` schneidet die akzeptierten Anforderungen in Tickets
  und schreibt den Zeitplan
- `/gg-plan-jira-export` exportiert die nächsten Tickets einer Person als
  CSV-Datei für Jira
- `/gg-plan-report` hält den Quartalsbericht jeder Person aktuell

Jedes Ticket der Familie bekommt eine Plandatei in
`doc/<yyyy>-Q<n>/tickets/` und nach Abschluss einen Blogbeitrag in
`doc/blog/<yyyy>/`. Anforderungen liegen in einer Datei pro Bereich mit
IDs `req-<area>-<nnn>`:

- [API](doc/requirements/api.md): die App-seitige API von `aud_midi`
- [Messages](doc/requirements/messages.md): typisierte Nachrichten,
  Codecs, Übersetzung und Konstanten von `aud_midi_standard`
- [Engine](doc/requirements/engine.md): Threading, Timing, Diagnostik und
  der Backend-Vertrag von `aud_midi_core`
- [Ports](doc/requirements/ports.md): Hotplug, Identität, virtuelle Ports
  und Fähigkeiten
- [Network](doc/requirements/network.md): AppleMIDI-Sessions, das
  RTP-MIDI-Journal, Discovery und Network MIDI 2.0
- [Bluetooth](doc/requirements/bluetooth.md): die App als
  Bluetooth-LE-Central und -Peripheral
- [Platforms](doc/requirements/platforms.md): was jedes Betriebssystem
  bietet und ab welcher Version
- [State](doc/requirements/state.md): die Noten-Matrix und die
  Controller-Matrix von `aud_midi_state_matrix`
- [Constraints](doc/requirements/constraints.md): feste Technologien,
  Package-Regeln und Qualitätsschranken

## Dokumentation

Quartal 2026-Q4:

- [Tickets](doc/2026-Q4/tickets)
- [Entscheidungen](doc/2026-Q4/concepts/decisions/000-index.md)
- [Architektur](doc/2026-Q4/architecture/architecture.md)
- [Szenarien: die Menschen und die Hauptszenarien der Familie](doc/2026-Q4/concepts/topics/scenarios.md)
- [Plan aud_midi_01: Initiale MIDI-Implementierung](doc/2026-Q4/tickets/2026-10-06-aud_midi_01-initial-midi-implementation.md)
- [Plan aud_midi_02: Aktive Noten verfolgen](doc/de/2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md)

Quartalsübergreifend:

- [Referenzen: Spezifikationen und Referenzimplementierungen](doc/references/README.md)
- [Blog](doc/de/blog/)
- [Guides](doc/guides/)

## Mitwirken

Siehe [doc/guides/develop-guide.md](doc/guides/develop-guide.md) und den
[Guide zum Projektmanagement-Repo](doc/guides/pm-repo-guide.md). Das Repo
führt zwei Sprachen: Englisch unter `doc/`, Deutsch unter `doc/de/`, siehe
[doc/guides/translate-guide.md](doc/guides/translate-guide.md).

## Lizenz

MIT — siehe [LICENSE](LICENSE). Die Dateien unter `doc/references/`
behalten ihre eigenen Lizenzen, aufgeführt in
[doc/references/README.md](doc/references/README.md).
