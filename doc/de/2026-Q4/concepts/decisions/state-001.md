# state-001: Notenzellen sind Stapel von Velocity-Slots

- Status: accepted
- Date: 2026-10-27
- Canonical source: Ticket aud_midi_02, Entscheidung des Nutzers bei der
  Planung
- Open work: Reset All Controllers (CC 121), quellenbezogenes Loslassen,
  MIDI-2.0-Nachrichten

`MidiNoteMatrix` hält pro Kanal und Note eine Zelle mit Velocity-Slots,
anfangs fünf, bei Bedarf verdoppelt bis zehn, 0 bedeutet frei. Ein Note
On schreibt seine Velocity in den ersten freien Slot, ein Note Off gibt
den zuletzt belegten Slot frei, so sind die belegten Slots immer
lückenlos von vorn gefüllt und ihre Zahl sagt, wie viele Note Ons die
Note halten. Eine volle Zelle zählt kein weiteres Note On; die neueste
Velocity ersetzt den zuletzt belegten Slot, so dass ein Strom von Note Ons
ohne Note Offs die Matrix nicht wachsen lässt. In Richtung `midiOut`
sendet die Matrix das erste Note On, nichts für die Note Offs, die die
Note gehalten lassen, und das letzte Note Off wie empfangen. Ein
wiederholtes Note On sendet ein Note Off mit Release-Velocity 64 und
danach das neue Note On, solange `retriggerRepeatedNoteOns` für den Kanal
wahr ist, und nichts, solange es falsch ist. Ein Note On mit Velocity 0
gilt als Note Off. Ein Note Off ohne gehaltene Note wird unverändert
weitergereicht, so dass hinter der Matrix keine Note hängen bleiben
kann. All Sound Off, All Notes Off und die Mode-Nachrichten, die es
einschließen (CC 120, 123 bis 127), senden für jede gehaltene Note ihres
Kanals ein Note Off und laufen danach durch; `release`, `releaseChannel`
und `releaseAll` tun dasselbe auf Zuruf, so dass die App ein Note Off
nachholen kann, das nie ankommt. Alle anderen Nachrichten laufen
unverändert und in Reihenfolge durch, so dass `MidiNoteMatrix` und
`MidiControllerMatrix` als Kette vor einem Ausgang stehen können.
