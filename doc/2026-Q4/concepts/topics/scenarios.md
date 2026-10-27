# Scenarios

The people of the product and the main scenarios the aud_midi family
serves, agreed with the user on 2026-10-27 in ticket aud_midi_03. The
requirements trace back to this topic.

## People

- Audanika's app developers use the packages, directly and through the
  `aud_audio` engine; they are the primary users
- Musicians with MIDI gear are the end users of those apps: keyboards,
  controllers, synths, DAWs. They never see the packages, but timing and
  stuck notes hit them
- Dart developers outside Audanika use the packages from GitHub or
  pub.dev for their own MIDI software
- Peer apps and devices are affected, not users: DAWs, synths and other
  apps that exchange MIDI with the app over virtual ports, Bluetooth LE
  or the network, and the OS MIDI services in between
- Audanika pays. Nobody operates a library; the app developer integrates
  it and sets the platform permissions

## Plug in a USB keyboard

1. The musician plugs a USB keyboard into the phone, tablet or
   computer.
2. Within a second the port list of the app shows the input and the
   output of the keyboard.
3. The app subscribes to the input; every message arrives with the time
   the operating system stamped.
4. The app sends notes to a synth connected the same way.
5. The musician unplugs and re-plugs the keyboard; the app finds it
   again as the same port.

## Play a sequence while the UI is busy

1. The app schedules the notes of the next bar with due times.
2. The UI animates, loads a file and rebuilds frames meanwhile.
3. Every note leaves the port at its due time; the musician hears no
   jitter.
4. The musician stops; notes not yet due are cancelled and a panic
   silences everything that still sounds.
5. When the app cannot keep up, a queue overflows: the app receives a
   diagnostic and the stream goes on.

## Join a session over Wi-Fi

1. The app lists the AppleMIDI sessions advertised on the local network:
   a Mac, a Windows PC with rtpMIDI, another tablet.
2. The musician picks one; the app joins and the session appears as an
   input and an output.
3. Notes cross the network; a lost packet is repaired from the recovery
   journal and no note sticks.
4. The app offers its own session; a DAW on another machine connects to
   it.
5. Network MIDI 2.0 works the same way with UMP, with authentication
   when the host asks for it.

## Connect a Bluetooth LE keyboard

1. The musician switches on a BLE-MIDI keyboard; the app scans and
   lists it.
2. The app connects; the keyboard becomes an input and an output port.
3. Notes arrive in order with the timestamps of the BLE-MIDI packets.
4. The connection drops out of range; the port disappears and returns
   when the keyboard is back.
5. The app advertises itself as a BLE-MIDI peripheral; an iPad connects
   to it and plays.

## Appear as a virtual port

1. The app creates a virtual source and a virtual destination carrying
   its name.
2. Another app on the same device, a DAW or a synth app, lists them and
   connects.
3. Messages flow in both directions.
4. The virtual ports disappear when the app closes MIDI.

## Use a MIDI 2.0 device

1. A UMP endpoint, a MIDI 2.0 keyboard on macOS 15 or Android 13+,
   appears as a port with its protocol and its groups.
2. The app receives MIDI 2.0 channel voice messages with 16-bit velocity
   and per-note controllers.
3. The app sends the same messages to a MIDI 1.0 synth; the translation
   scales and folds them as the UMP specification says.
4. A message without a MIDI 1.0 equivalent is dropped and the app
   receives a diagnostic.

## Merge two inputs into one output

1. A keyboard and a sequencer play into the app; both play C4.
2. The note matrix sends one Note On; the second one is counted, not
   sent, unless the channel retriggers.
3. The keyboard releases C4: nothing goes out. The sequencer releases
   it: the Note Off goes out.
4. The app reads which notes sound and the current value of every
   controller for its display.
5. On stop the app releases all: a Note Off for every sounding note.

## Test without hardware

1. A developer writes a test for the MIDI code of their app.
2. The test runs against a fake backend with scripted ports and
   messages, on any machine.
3. The same contract tests prove every backend of the family.
4. CI runs the checks of every package on macOS, Linux and Windows.
