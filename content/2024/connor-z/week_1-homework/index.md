---
title: Week_1 Homework
date: 2024-10-28T00:02:00.000Z
authors:
  - Connor Z
image: feature.jpg.jpg
bgimage: background.jpg
showBgImage: true
---
**First Arduino Work
INSTRUCTION**
Create a circuit and Arduino code that does the following

**Circuit #**
Connect two LEDs to your Arduino using a breadboard
Connect one switch to your Arduino using a breadboard
**Code #**
Read a momentary switch being pressed
When the program starts, both LEDs are off
When the switch is pressed once, the first LED turns on
When the switch is pressed the second time, the second LED turns on (the first one should also still be on)
When the switch is pressed the third time, both LEDs turn off
Repeat this same cycle of LEDs turning on and off in sequence (off, one LED, two LEDs, off…)

Circuit

![2 LED and 1 BUTTON](w1.png)

Connecting the second LED is not hard, basically I just copied how LED 1 is connected. 

Firse LED - Connected to Port 4

Second LED - Connected to Port 8

![Actual Attempt](whole.jpg)

And it works. 

I tested it out by activating both LED when I pressed the button. All are connected. 

(Except the first time I put the wrong port on code and confused about why it isn't work)
