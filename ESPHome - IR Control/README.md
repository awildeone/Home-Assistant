# ESPHome IR Control

I currently have an older TV that doesn't support any network, serial, or RS232 control. But it has IR.
As I continue to work on feature parity between Control4 and HomeAssistant in my home, having IR control is almost a must. So many devices use IR control becuase there are lots of well developed IR control standards that companies share. For example, my Vizio TV uses the NEC control standard. While my Kanto TUK speakers use RC5. Researching IR standards is a black hole, good luck.

# Setup Process

I didnt (currently) want to have IR recieve (RX) on my device because my HID inputs are ALWAYS* going to come from a HA Dashboard. So the need to send RX into HA wasn't needed. So how did I find my IR codes? Arduino.

I used an Arduino Uno to setup a RX diode with an included IR Sketch that is built into Arduino IDE. Once that was hooked up, I opened the serial console monitor and pressed a button on the TV remote. It dumped out TONS of information that I didnt need. But what I did need, was the raw data output. You can use the +/- values but I wanted something cleaner as the ESPHome documentation does show 8-bit, 16-bit, and 32-bit options.

What I found was that the way the NEC code is combined has two 16-bit codes (tada, maths) in the same line. The notes on this are in the YAML file, but I'll post is here too:

```
address: "0xFB04"
command: "0xF708"
---
Raw-data: 0xF708FB04
The first 4 characters are the command
The last 4 characters are the address
```

My understanding is that the address is the device itself. Whenever you send a code to the TV, the address is always the same. So if you were building more than just one code, you could just copy/paste template YAML, and then just update the command. I'm not an expert, but as I pressed each button, this theory holds true.

# HA Control

I only wanted power toggle button, so thats what I made, and its what the YAML shows. This can be setup as other interface options than a button, but in terms of how you'd want to trigger this in HA, a button made the most sense.

## ALWAYS*

So as I was working on this project, my Logitech Harmony 665 sat on my desk looking back at me asking "When will you use me again"?

Now while Logitech has killed their harmony remote series (sad), they still support IR learning. If I setup IR codes for HA automations and script triggers, *technically* I could use my Harmony remote to control Home Assistant. Maybe this is a future project.
