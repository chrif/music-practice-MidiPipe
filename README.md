# Music Practice MidiPipe

## MidiPipe implementation for using the buttons of 2 Yamaha Piaggero NP-12 as generic MIDI controls

### Required virtual ports (from IAC driver):

- MidiPipe
- Buttons as Notes
- B1
- B2
- B3
- B4
- B5

### Channel conventions

- Channel 2    Program Changes from Swell converted to Notes
- Channel 3    Program Changes from Great converted to Notes
- Channel 16   Used between pipe tools for filtering out messages

### Piping

##### Abstract pipes first process buttons

- *Program Changes to Notes* pipe:
  - Takes input from Swell (physical port DIN3) and Great (physical port DIN4)
  - Converts Program Changes to Notes
  - Routes Swell to channel 2
  - Routes Great to channel 3
  - Outputs to virtual port *Buttons as Notes*

- The following *Button* pipes then take input from virtual port *Buttons as Notes*
```
Button 1       filters out correct Note and converts it to Control Change 0 127 on virtual port B1
Button 2 On    filters out correct Note and converts it to Control Change 0 127 on virtual port B2
Button 2 Off   filters out correct Note and converts it to Control Change 1 127 on virtual port B2
Button 3       filters out correct Note and converts it to Control Change 0 127 on virtual port B3
button 4 On    filters out correct Note and converts it to Control Change 0 127 on virtual port B4
Button 4 Off   filters out correct Note and converts it to Control Change 1 127 on virtual port B4
Button 5       filters out correct Note and converts it to Control Change 0 127 on virtual port B5
```

##### The concrete pipes then

1. Take input from the desired button port (B1 to B5)
2. Keep only channel 2 or 3 to select button's manual (Swell is channel 2, Great is channel 3).
3. Converts the channel and Control Change to what Hauptwerk expects
4. Output to virtual port *MidiPipe* which Hauptwerk listens to

### Notes

- Buttons 2 and 4 have 2 states and can as act toggle buttons if needed.