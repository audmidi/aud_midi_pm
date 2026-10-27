# aud_midi_02: Aktive Noten verfolgen

## Ziel

Apps bekommen einen Ort, der weiß, welche MIDI-Noten gerade klingen und
welche Controller-Werte aktuell sind. Das neue Paket
`aud_midi_state_matrix` hält beides als Matrizen aus 16 Kanälen × 128
Noten bzw. Controllern. Die Notenmatrix merkt sich jedes Note On einer
Note in einem Velocity-Slot, so dass mehrere Note Ons derselben Note
(zwei Finger, zwei zusammengeführte Eingänge) gezählt werden und erst das
letzte Note Off die Note zum Ausgang hin loslässt. Die Controller-Matrix
hält den letzten Wert jedes Controllers pro Kanal. Beide stehen über
einen `midiOut`-Callback als Kette vor einem MIDI-Ausgang.

## Requirements

Erarbeitet in Ticket aud_midi_03 und gehalten in
[requirements/state.md](../../../requirements/state.md). Dieses Ticket
erfüllt req-state-001 bis req-state-006 und den Notenmatrix-Teil von
req-state-007 (All Sound Off, All Notes Off und die Mode-Nachrichten, die
es einschließen). Noch offen: Reset All Controllers in der
Controller-Matrix (req-state-007) und MIDI-2.0-Noten und -Controller
(req-state-008).

## Betroffene Repos

| Repo | Änderung |
| --- | --- |
| `aud_midi_pm` | dieser Plan, Entscheidung state-001, eine Zeile in Architektur und Package-Graph |
| `aud_midi_standard` | die geteilten Typen `MidiKey` (Kanal und Note einer Notennachricht) und `MidiMessageCallback` (Empfänger von Nachrichtenlisten) |
| `aud_midi_state_matrix` | neues Repo: `MidiNoteMatrix`, `MidiControllerMatrix`, Beispiel, READMEs, `index.jsonc` |

Das neue Paket ist reines Dart und hängt nur an `aud_midi_standard`
(`MidiMessage`, `MidiNoteOn`, `MidiNoteOff`, `MidiControlChange`,
`MidiKey`, `MidiMessageCallback`). Gemeinsames Vokabular liegt in
`aud_midi_standard`, nicht in einem eigenen Repo.

## Entwurf

### MidiNoteMatrix

- 16 Kanäle × 128 Noten. Jede Zelle hält Velocity-Slots, anfangs fünf
  (`initialSlots`), der Wert 0 bedeutet frei. Sind alle Slots einer Zelle
  belegt, verdoppelt die Zelle ihre Slots, bis zu `maxSlots` (Standard
  10). Eine volle Zelle zählt kein weiteres Note On: die neueste Velocity
  ersetzt den zuletzt belegten Slot, so dass ein Strom von Note Ons ohne
  Note Offs die Matrix nicht wachsen lässt.
- Eine Zelle ist ein Stapel: ein Note On schreibt seine Velocity in den
  ersten freien Slot, ein Note Off gibt den zuletzt belegten Slot frei.
  Belegte Slots sind immer lückenlos von vorn gefüllt, der erste freie
  Slot ist immer `count` (state-001).
- `midiIn(List<MidiMessage>)` verarbeitet Note On und Note Off; ein Note
  On mit Velocity 0 gilt als Note Off. Jede andere Nachricht läuft
  unverändert und in Reihenfolge durch, so dass die Matrix in einer Kette
  stehen kann.
- `midiOut` ist ein `MidiMessageCallback` aus `aud_midi_standard`,
  `void Function(List<MidiMessage>)`. Jeder `midiIn`-Aufruf, der
  Nachrichten erzeugt, ruft ihn einmal mit allen in Reihenfolge.
- `retriggerRepeatedNoteOns(channel)` liest das Flag pro Kanal,
  `setRetriggerRepeatedNoteOns(channel:, value:)` setzt es, Standard
  `true`.
- `activeNotes` ist eine gepflegte Liste von `MidiKey(channel, note)` aus
  `aud_midi_standard`: ein Eintrag kommt dazu, wenn eine Zelle von 0 auf
  1 geht, und geht, wenn sie von 1 auf 0 geht. `velocities`, `slots`,
  `count` und `isActive` lesen eine Zelle.
- `reset()` leert still; `release(channel:, note:)`,
  `releaseChannel(channel)` und `releaseAll()` senden für jede gehaltene
  Note in ihrem Bereich ein Note Off und geben die Zellen frei, so dass
  die App Noten beenden kann, deren Note Off nie ankommt.
- All Sound Off (CC 120), All Notes Off (CC 123) und die Mode-Nachrichten,
  die All Notes Off einschließen (CC 124 bis 127), lassen die gehaltenen
  Noten ihres Kanals auf dieselbe Weise los und laufen nach den Note Offs
  durch.

Die Senderegeln pro Zelle, `n` ist die Zahl belegter Slots vor der
Nachricht:

| Eingang | Zustand | Retrigger an | Retrigger aus |
| --- | --- | --- | --- |
| Note On | n = 0 | Note On senden | Note On senden |
| Note On | n ≥ 1 | Note Off (Release 64), dann das neue Note On | nichts |
| Note Off | n ≥ 2 | nichts | nichts |
| Note Off | n = 1 | das Note Off senden, wie empfangen | wie links |
| Note Off | n = 0 | unverändert weiterreichen (keine Hänger) | wie links |

### MidiControllerMatrix

- 16 Kanäle × 128 Controller, pro Zelle der letzte Wert 0 bis 127,
  `null`, bis ein Wert gesetzt wurde.
- Gleiche Form wie die Notenmatrix: `midiIn` merkt sich jeden Control
  Change und reicht alle Nachrichten unverändert weiter. `value`,
  `isSet`, `set`, `reset`, `resetChannel` und `entries` (eine Liste von
  `MidiControlChange`, bereit zum Senden) lesen und ändern den Zustand.
- Nicht in diesem Ticket: 14-Bit-Paare (MSB/LSB), RPN/NRPN und eine
  Sonderbehandlung von Reset All Controllers (CC 121).

## Schritte

1. `aud_midi_pm` ins Ticket nehmen, diesen Plan und state-001 schreiben.
2. `aud_midi_state_matrix` mit `gg do create repo` anlegen, die
   Abhängigkeit auf `aud_midi_standard` eintragen.
3. `MidiKey` und `MidiMessageCallback` mit Tests in `aud_midi_standard`
   ergänzen; `MidiNoteMatrix` mit Tests umsetzen.
4. `MidiControllerMatrix` mit Tests umsetzen; ihre `entries` sind
   `MidiControlChange`.
5. Beispiel, README in Englisch und Deutsch, `index.jsonc`,
   Publish-Config.
6. `gg can commit` grün mit 100 % Abdeckung, ein Commit pro Repo; Push,
   Review und Publish auf Zeichen des Nutzers.

## Offene Fragen

Am 2026-10-27 mit dem Nutzer entschieden und in
[state-001](../concepts/decisions/state-001.md) festgehalten: ein Note Off
gibt den zuletzt belegten Slot frei; ein Note Off ohne gehaltene Note wird
weitergereicht; fremde Nachrichten laufen durch; der Eingang heißt
`midiIn`; die Controller-Matrix wird in diesem Ticket umgesetzt. Ebenfalls
am 2026-10-27 entschieden: die geteilten Typen wandern als `MidiKey` und
`MidiMessageCallback` nach `aud_midi_standard` statt in ein eigenes Repo,
und die Einträge der Controller-Matrix sind
`MidiControlChange`-Nachrichten.

Noch offen, für spätere Tickets:

- Reset All Controllers (CC 121) wird wie jeder Wert gespeichert; die
  Controller-Matrix interpretiert ihn nicht.
- Hängende Noten jenseits der Channel-Mode-Nachrichten und der
  Release-Hooks: die Quelle jeder gehaltenen Note merken
  (`midiIn(messages, source:)`, `releaseSource(source)`), damit ein
  verschwundener Port oder eine beendete Touch-Sitzung nur die eigenen
  Noten loslässt, und ein optionales Zeitlimit, das die App treibt
  (`sweep(now)`, `maxHold` pro Kanal).
- MIDI-2.0-Channel-Voice-Nachrichten (`MidiNoteOn2`, `MidiControlChange2`,
  …) laufen durch; vorher mit `toMidi1()` übersetzen.
