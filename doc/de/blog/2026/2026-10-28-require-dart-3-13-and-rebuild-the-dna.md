<!--
@license
Copyright (c) audanika

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Dart 3.13 voraussetzen und die DNA neu bauen

## Motivation

- Die MIDI-Pakete werden in einem Durchgang gepflegt: Jedes Repo
  wechselt auf das aktuelle Dart SDK, übernimmt die aktuellen Versionen
  seiner Abhängigkeiten und baut seine DNA-Instanzen neu.
- Dieses Repo hält die Pläne, Entscheidungen und Posts der Familie,
  keinen Code. Sein Anteil am Durchgang ist die SDK-Untergrenze, gegen
  die seine Dev-Abhängigkeiten aufgelöst werden, und die DNA-Instanzen,
  die den Planungs-Workflow steuern.
- Die Instanzen zählen hier mehr als anderswo: `CLAUDE.md` und die
  Guides sind das, was eine KI liest, bevor sie in diesem Repo ein Ticket
  plant. Sie müssen also aus der Engine kommen, mit der das Repo sie
  testet.

## Strategie

- Die SDK-Bedingung auf `^3.13.0` anheben, im Gleichschritt mit den
  Paketen, die die Pläne beschreiben.
- `gg do upgrade deps` bestätigen lassen, dass `dna_audanika`, `dna_plan`
  und `helix` aktuell sind; das waren sie, die Lock-Datei bleibt.
- Die DNA mit `gg dna build` neu bauen und den DNA-Test des Repos das
  Ergebnis prüfen lassen, wenn die Checks laufen.

## Realisierung 1: SDK-Untergrenze

- `pubspec.yaml`: `environment.sdk` wechselt von `^3.12.2` auf
  `^3.13.0`.
- Sonst ändert sich nichts am Manifest; das Repo wird nicht
  veröffentlicht.

## Realisierung 2: DNA-Instanzen

- `gg dna build` von gg 17.6 hat die Instanzen mit der helix-1.13-Engine
  gebaut, die gg mitbringt: Es hat `.claude/CLAUDE_dna_gg.md` und
  `.claude/CLAUDE_dna_plan.md` in `CLAUDE.md` eingefaltet und das als
  `#gg: generated DNA` committet.
- Der DNA-Test des Repos läuft mit `helix` 1.14, der Dev-Abhängigkeit,
  und hat die Instanzen beim nächsten Check erneut gebaut: Die beiden
  Dateien kamen zurück, `CLAUDE.md` verweist wie zuvor auf sie und das
  Manifest hält 1.14 als Engine fest. Das landete als zweiter Commit
  `#gg: generated DNA`.
- Die beiden Commits heben sich auf: Guides, Skills und `CLAUDE.md` des
  Repos sind die, die es vorher hatte, geprüft von seiner eigenen
  Engine.

## Aktualisierte Abhängigkeiten

- Keine Abhängigkeit hat ihre Version geändert. `dna_audanika` 0.1.0,
  `dna_plan` 1.0 und `helix` 1.14 waren bereits aktuell.

## Offene Punkte

- gg 17.6 baut die DNA mit helix 1.13, während das Repo sie mit helix
  1.14 testet. Bis gg auf 1.14 wechselt, sind sich `gg dna build` und
  der DNA-Test über `CLAUDE.md` uneins; der Test hat das letzte Wort.
- Die DNA-Variablen dieses Repos nennen `audanika` als
  Git-Organisation, während das Repo unter `audmidi` liegt. Die
  Badge-URLs der README-Vorlage zeigen dorthin. Ein späterer Durchgang
  über die MIDI-Familie sollte `dnaGitOrg` und `dnaGitOrgUrl` auf
  `audmidi` setzen.
