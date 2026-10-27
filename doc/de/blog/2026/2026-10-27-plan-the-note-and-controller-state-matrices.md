# Die Noten- und Controller-Zustandsmatrizen planen

## Motivation

- Das Ziel „Apps wissen, welche Noten klingen“ braucht in der Familie
  einen Ort, der den Zustand hält, den ein MIDI-Strom hinterlässt: die
  klingenden Noten mit ihren Velocities und die aktuellen
  Controller-Werte.
- Ticket aud_midi_02 plant diesen Ort als eigenes Paket und hält die
  Entscheidungen auf dem Weg fest, damit spätere Tickets und die
  Anforderungen darauf aufbauen können.

## Strategie

- Das Ticket zuerst in diesem Repo planen: Ziel, betroffene Repos,
  Entwurf, Schritte und offene Fragen in einer Plan-Datei.
- Die Slot-Semantik der Notenmatrix als Entscheidung state-001 festhalten
  und das Paket in Architektur und Package-Graph eintragen.
- Die offenen Fragen vor der Umsetzung mit dem Nutzer entscheiden und die
  Antworten in Plan und Entscheidung übernehmen.
- Den Plan lebendig halten, während die Umsetzung von ihm abweicht.

## Realisierung 1: der Plan

- `doc/2026-Q4/tickets/2026-10-27-aud_midi_02-track-active-notes.md`
  beschreibt die zwei Matrizen, die Senderegeln pro Zelle als Tabelle und
  die Schritte des Tickets.
- Die betroffenen Repos: `aud_midi_pm` für den Plan, `aud_midi_standard`
  für die geteilten Typen und das neue `aud_midi_state_matrix` für die
  Matrizen.
- Der Abschnitt zu den Anforderungen verweist auf `requirements/state.md`
  aus Ticket aud_midi_03; das Ticket erfüllt req-state-001 bis
  req-state-006 und den Notenmatrix-Teil von req-state-007, die auf done
  gesetzt sind.

## Realisierung 2: Entscheidung state-001

- Notenzellen sind Stapel von Velocity-Slots: ein Note On füllt den
  ersten freien Slot, ein Note Off gibt den zuletzt belegten frei, so sagt
  die Zahl, wie viele Note Ons die Note halten.
- Zum Ausgang gelangen nur das erste Note On und das letzte Note Off; ein
  wiederholtes Note On schlägt je nach Kanaleinstellung neu an; ein Note
  Off ohne gehaltene Note und jede fremde Nachricht laufen durch.
- Gemeinsames Vokabular wandert als `MidiKey` und `MidiMessageCallback`
  nach `aud_midi_standard` statt in ein eigenes Repo; die Einträge der
  Controller-Matrix sind `MidiControlChange`-Nachrichten.

## Realisierung 3: Änderungen während des Tickets

- Eine Zelle zählt höchstens `maxSlots` (10) Note Ons, damit endlose Note
  Ons die Matrix nicht wachsen lassen.
- Release-Pfade für hängende Noten: `release`, `releaseChannel`,
  `releaseAll` und die Channel-Mode-Nachrichten, die Noten beenden.
- Die Architektur führt jetzt elf Pakete; der Package-Graph zeigt
  `aud_midi_state_matrix` auf `aud_midi_standard`.
- Das Tickets-README und das Repo-README verlinken den Plan.

## Aktualisierte Abhängigkeiten

- `dna_audanika` 0.1.0: Blog- und Übersetzungs-Guides, ein Prompt-Hook
  und ein README-Prüfskript; die DNA fragt nach dem Namen der
  Quick-Check-Pipeline, `Quick Check`.

## Offene Punkte

- Ein Folgeticket für das quellenbezogene Loslassen hängender Noten und
  ein optionales Zeitlimit, im Plan vermerkt.
- Deutsche Kopien der Ziele, Anforderungen und älteren Pläne; dieses
  Ticket übersetzt nur die Dateien, die es anlegt.
