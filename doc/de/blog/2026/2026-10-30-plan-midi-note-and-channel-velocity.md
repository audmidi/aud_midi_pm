<!--
@license
Copyright (c) audanika

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# MidiNote und ChannelVelocity planen

## Motivation

- Das Ziel „Apps wissen, welche Noten klingen" braucht eine Note mit ihrer
  Velocity und eine Stimme, einen Kanal mit einer Velocity, als
  gemeinsame Werte. `MidiKey` deckt nur Kanal und Notennummer ab.
- Keine akzeptierte Anforderung deckte solche Werte ab, also begann das
  Ticket `midi_note_and_json_parsing` mit Requirements Engineering, wie es
  der PM-Repo-Guide verlangt.
- Das Ticket wollte zuerst auch den JSON-Code der Modelle mit
  `json_serializable` erzeugen. Das stieß auf eine `must`-Anforderung und
  wurde gestrichen; dieser Post hält beide Entscheidungen fest.

## Strategie

- Die Anforderungen mit dem Nutzer erarbeiten, eine Frage nach der
  anderen, und nur aufschreiben, was bestätigt ist.
- Das Ticket auf der akzeptierten Anforderung planen, mit den betroffenen
  Repos, den Entwurfsentscheidungen und den Schritten.
- Den Plan lebendig halten: Als der Umfang schrumpfte, folgten Plan und
  Anforderungen im selben Ticket.

## Umsetzung 1: Anforderung req-messages-009

- Neu in `doc/requirements/messages.md`: „Notes and voices as values",
  Priorität `must`. Eine Note, ihr Kanal, ihre Notennummer und ihre
  Velocity, und eine Stimme, ein Kanal und eine Velocity, gibt es als
  unveränderliche Werte, die nach Wert vergleichen.
- Abnahme: Ein Programm, das nur von `aud_midi_standard` abhängt, baut
  aus einer `ChannelVelocity` und einer Notennummer eine `MidiNote`, liest
  ihren `MidiKey` und vergleicht zwei Noten nach Wert; beide überstehen
  `toJson` und `fromJson`.
- Das Ziel „Apps wissen, welche Noten klingen" verlinkt die Anforderung.

## Umsetzung 2: der Plan

- `doc/2026-Q4/tickets/2026-10-30-midi_note_and_json_parsing-add-midi-note-and-channel-velocity.md`,
  auf Englisch und Deutsch, verlinkt aus dem Ticket-README.
- `MidiNote` ist komponiert, nicht von `MidiKey` abgeleitet: `MidiKey` ist
  eine final class und vergleicht `other is MidiKey`, eine Unterklasse
  würde also unsymmetrisch vergleichen.
- `ChannelVelocity.note(int)` erzeugt die Note einer Notennummer.
- Release 0.3.1 von `aud_midi_standard`: Nichts Bestehendes ändert sich,
  die Pakete auf `^0.3.0` brauchen also kein Release.

## Umsetzung 3: json_serializable gestrichen

- Erzeugter JSON-Code braucht `json_annotation` zur Laufzeit. Das bricht
  req-constraints-010, die `aud_midi_standard` allein auf dem Dart SDK
  hält.
- Eine geänderte req-constraints-010 und eine neue Anforderung für
  kompatibles JSON wurden geschrieben und am selben Tag zurückgenommen,
  als der Nutzer den JSON-Teil strich. Beide Anforderungen stehen wie vor
  dem Ticket.
- Die Ticket-ID trägt noch „json_parsing"; der Plan erklärt, warum.

## Aktualisierte Abhängigkeiten

- Nur Entwicklungswerkzeuge: Die DNA-Schicht und der Analyzer sind auf
  ihre neuesten Patch- und Minor-Versionen gestiegen.

## Offene Punkte

- req-messages-009 auf `done` setzen, sobald `aud_midi_standard` 0.3.1
  veröffentlicht ist.
