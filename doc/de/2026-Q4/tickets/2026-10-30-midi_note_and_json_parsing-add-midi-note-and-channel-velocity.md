# midi_note_and_json_parsing: MidiNote und ChannelVelocity hinzufügen

## Ziel

Apps, die Noten rendern, brauchen eine Note mit ihrer Velocity und eine
Stimme, einen Kanal mit einer Velocity, als gemeinsame Werte neben
`MidiKey`. `aud_midi_standard` bekommt beide. Das Ticket dient dem Ziel
[Apps wissen, welche Noten klingen](../../../goals/apps-know-which-notes-sound.md).

Das Ticket sah zuerst auch vor, den JSON-Code der Modelle mit
`json_serializable` zu erzeugen. Der Nutzer hat das am 2026-10-30
gestrichen: Die Modelle behalten ihre handgeschriebenen Leser, und
`aud_midi_standard` hängt weiter nur vom Dart SDK ab
([req-constraints-010](../../../requirements/constraints.md#req-constraints-010-thin-dependencies-of-the-building-blocks)).

## Anforderungen

- [req-messages-009: Notes and voices as values](../../../requirements/messages.md#req-messages-009-notes-and-voices-as-values),
  mit dem Nutzer am 2026-10-30 erarbeitet

## Betroffene Repos

| Repo                | Änderung                                    |
| ------------------- | ------------------------------------------- |
| `aud_midi_pm`       | req-messages-009, dieser Plan               |
| `aud_midi_standard` | `MidiNote`, `ChannelVelocity`; Patch, 0.3.1 |

## Entwurf

- `MidiNote`: final class mit `channel` (0 bis 15), `note` und `velocity`
  (0 bis 127), geprüft wie bei `MidiKey`; `MidiKey get key`, `copyWith`,
  `fromJson` und `toJson` mit den Schlüsseln `channel`, `note` und
  `velocity`, Gleichheit nach Wert. Sie ist komponiert, nicht von
  `MidiKey` abgeleitet (entschieden am 2026-10-30): `MidiKey` ist eine
  final class und vergleicht `other is MidiKey`, eine Unterklasse würde
  also unsymmetrisch vergleichen
- `ChannelVelocity`: final class mit `channel` und `velocity`;
  `MidiNote note(int note)` erzeugt die Note einer Notennummer;
  `copyWith`, JSON, Gleichheit nach Wert
- JSON mit `MidiJsonReader`, wie bei `MidiKey`
- Version 0.3.1: Die Klassen sind neu, nichts ändert sich, daher behalten
  `aud_midi_state_matrix`, `aud_midi_core`, `aud_midi`, `aud_midi_network`
  und `aud_midi_rtp` `^0.3.0` und brauchen kein Release

## Schritte

1. req-messages-009 und dieser Plan (2026-10-30)
2. `MidiNote` und `ChannelVelocity` mit Tests
3. README auf Englisch und Deutsch, `index.jsonc`, Publish-Config (Patch)

## Offene Fragen

Keine.
