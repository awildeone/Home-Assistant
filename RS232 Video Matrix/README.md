# RS232 Video Matrix Control
This project was set up as I have Sonos devices for eARC audio and do not want to use an AV Receiver anywhere (for now). There are lots of 4in 2out, 4in 4out, etc, serial-controlled matrixes on Amazon. These just use phoenix connectors with RS232 or TRS 3.5mm on the matrix. So with ESP Home and a couple of wires, the matrix can be controlled through Home Assistant. This was also a development project for how I can control expanded devices outside of Home Assistant. The goal is to have better feature parity between Home Assistant and Control4.

## Features:
- All inputs are a unique button to each output (so with a 4x2 matrix, there are 12 buttons since "ALL OUT" is the third output option).
- Buttons are "single triggers" so there is no exclusive swtich options needed.
- Logging is reported into ESP Home as well as Home Assistant for history
- Multiple Serial Devices can be controlled through the ESP8266/ESP32. YMMV depending on which ESP you are using.
- Easy to adapt to bigger matrixes

## Keypad Card Example
![Matrix Keypad](https://github.com/awildeone/Home-Assistant/blob/main/RS232%20Video%20Matrix/Matrix%20Grid%20Example.png)


## Things I've Learned
- You have to include "character return" or "line feed" in double quotes for each uart.write command for it to send. Eg: uart.write: "This is foo\r\n".
  - It should be noted that you can type \r\n for character return. There are also hex value options that are included standard in YAML documentation. But thats for your own googling. All of mine are strings so using the above is my solution.
- You need to use a level shifter (anything with a MAX3232 chip). RS232 is NOT the same voltage as TTL. While the config is the same, RS232 requires +/- 9v for control. TTL at max is 5v only.
