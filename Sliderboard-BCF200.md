# Setting Device to USB Mode 1 (U-1)
This is a copy from (here)[http://www.freestylersupport.com/wiki/external_control:midi:using_bcf2000]

USB mode 1 (U-1) is used for controlling software via USB (like FreeStyler) and disables all the MIDI ports on the actual BCF2000.

Note: If this is not your desired setup, please refer to the BCF2000 user manuals for setting your desired mode of operation.

   * Push & hold the EDIT button on the BCF2000, and press STORE button at the same time.
   * Release both buttons
   * Select U-1 by turning the “PUSH ENCODER 1” (uppermost left corner knob) until “U-1” is shown in the display area.
   * Push the EXIT button

**Always use the U1 mode!** This will create one input and one output midi device on the computer. This setting will be remembered when the slider board is turned off.

# Button indices

![Button layout](http://wcms.inf.ed.ac.uk/ipab/slmc/resources/2015-11-02%2015.29.55-1.jpg)

![Slider board](http://wcms.inf.ed.ac.uk/ipab/slmc/resources/em_bhr-bcf2000_top.jpg)

# Encoder groups and presets

You can switch between 4 encoder groups using the buttons on the top right of the slider board. This will allow you to set 24 virtual encoder values and use 24 virtual encoder buttons using just 8 physical encoders. The currently active encoder group button will always light up.

**DO NOT CHANGE THE PRESET!!!** 
Presets are used to assign each button a different function, e.g. assigning it a different index. Changing the preset will change the index of each control when it appears in your software!

# Reading and commanding the slider board
The slider board sends and receives standard MIDI commands. The programming interface may look different  depending on the operating system and programming language. In each case, the transferred data consists of a time stamp and 4 bytes of data: ```status (command type), data 1 (controller id), data 2 (controller value), data 3 (ignored)```.

Example message to receive:

```[2272093, 176, 81, 54, 0]``` - First fader on the left (ID=81) was set to value 54.

Example message to Send:

```[176, 81, 127, 0]``` - Set first fader on the left (ID=81) to value 127 (maximum).


The status should always be the magic number 176.
The time stamp and the last data byte may be omitted in some implementations.

# Implementation
 * SCS implements the MIDI interface through Java and allows for binding the faders to Yo variables (e.g. neck joint angle).
 * OpenHumanoids/DRC implements the MIDI interface through a Python library wrapper. Values can be read but writing has not been used.

Example python code (For DRC codebase):
```Python
import midi
midi.printDevices()
id_input = midi.findInputDevice('BCF2000 MIDI 1')
# Read out the id of the output device and manually assign it to id_output.
m=midi.MidiReader(id_input)
m.getMessages()
s=midi.pypm.Output(id_output,0)
s.Write([[[176,81,0],0]])
```