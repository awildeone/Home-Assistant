# RS232 Video Matrix Control
This project was set up as I have Sonos devices for eARC audio and do not want to use an AV Receiver anywhere (for now). There are lots of 4in 2out, 4in 4out, etc, serial-controlled matrixes on Amazon. These just use phoenix connectors with RS232 or TRS 3.5mm on the matrix. So with ESP Home and a couple of wires, the matrix can be controlled through Home Assistant. This was also a development project for how I can control expanded devices outside of Home Assistant. The goal is to have better feature parity between Home Assistant and Control4.

## Features:
- All inputs are a unique button to each output (so with a 4x2 matrix, there are 12 buttons since "ALL OUT" is the third output option).
- Buttons are "single triggers" so there is no exclusive swtich options needed.
- Logging is reported into ESP Home as well as Home Assistant for history
- Multiple Serial Devices can be controlled through the ESP8266/ESP32. YMMV depending on which ESP you are using.
- Easy to adapt to bigger matrixes since its just RS-232.

## Keypad Card Example
![Matrix Keypad](https://github.com/awildeone/Home-Assistant/blob/main/RS232%20Video%20Matrix/Matrix%20Grid%20Example.png)

# Parts
- ESP8266 or ESP32
  - [Amazon](https://www.amazon.ca/dp/B07S5Z3VYZ?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- MAX3232 RS-232 Level Shifter
  - [Amazon](https://www.amazon.ca/AiTrip-Converter-Equipment-Upgrades-Connecter/dp/B083L99CGZ/ref=cm_cr_arp_d_product_top?ie=UTF8])
- EZCoo 4x2 HDMI Matrix with RS-232
  - [Amazon](https://www.amazon.ca/dp/B083K39RHF?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- DB9 Breakout
  - [Amazon](https://www.amazon.ca/Connector-Adapter-Terminal-Signal-Module/dp/B071DS5GTW)
- Wire (Jumper wires with dupont makes testing much easier)
- TTL Serial interface (optional, for troubleshooting only)

# Guide

### Overview:
This guide does require some understanding of ESP Home and electronics wiring. Such as TTL uses 3.3v and/or 5v voltages. To learn more, harness your "Google-Fu" and research the following:
  
- RS-232 Operating voltage & communication
- DB9 wiring and pinout
- ESP32 and ESP8266 wiring and features (Using an actual hardware UART is better)
- Documentation on whatever RS-232 device you want to control. Remember: RTFM

## Wiring

This is where I'd put my wiring diagram... IF I HAD ONE
(I will..... eventually. Also, check the YAML for gpio pin numbers)

## Software

### ESP Home

1. Create a new device and follow prompts
2. You can initialize the device from the ESP Home Web interface, OR you can just download the YAML file in this repo and install that.
   - You WILL need to look through the config for which lines are already existing on your device (such as wifi, API encryption, etc). You'll have a hard time if you don't add those.
3. Open Home Assistant and configure the new ESP Home device with whatever name you gave your device in step 1. If you do have a different name, you'll need to update the YAML config for the matrix control card example file in this repo.
4. In HA, you will now have 12 buttons to control the 12 input options.
5. Verify your wiring. Sanity check my notes. God knows that initial connections aren't perfect every time. (Dont ask how much time this took up.....)
6. Open two windows: First, Home Assistant with the matrix button. Second, ESP Home with the device log open.
7. Press a button in Home Assistant. If you have everything setup correctly, you should see uart.readline send/received appear.

### Troubleshooting:

1. If you see CMD_ERR, or similar, check your code. If you changed the ESP YAML, you mistyped the data lines, and thats why it's not working.
2. You see nothing happen. You need to make sure Home Assistant and ESP Home can talk to each other
3. You only see "button:xxxx" 'Button Name' Pressed, or similar. Your connection over UART or RS-232 is not working. Check your wiring.
4. The ESP is not showing up as online or as an available device when connected to my computer. Welp, you should check your wiring cause if you sent voltage somewhere it shouldnt be, you might need a new ESP controller. Luckily it's easy, start again at step 1.
5. Have you turned it off and on?

-----
### Things I've Learned (and you could too)
- You have to include "character return" or "line feed" in double quotes for each uart.write command for it to send. Eg: uart.write: "This is foo\r\n".
  - It should be noted that you can type \r\n for character return. There are also hex value options that are included standard in YAML documentation. But thats for your own googling. All of mine are strings so using the above is my solution.
- You need to use a level shifter (anything with a MAX3232 chip). RS232 is NOT the same voltage as TTL. While the config is the same, RS232 requires +/- 9v for control. TTL at max is 5v only.
- The level shifters I bought have a weird quirk. The labels on the RX and TX labels are backward (normally TX goes to RX, etc). The level shifter is RX to RX, and TX to TX. Not hard, just required some continuity testing between the header pins and the MAX3232 chip.
