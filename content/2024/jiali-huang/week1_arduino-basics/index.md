---
title: week1_Arduino Basics
date: 2024-10-28T22:17:00.000Z
authors:
  - Jiali Huang
image: feature.jpg
bgimage: background.jpg
showBgImage: true
---
INSTRUCTURE:

Read a momentary switch being pressed

When the program starts, both LEDs are off

When the switch is pressed once, the first LED turns on

When the switch is pressed the second time, the second LED turns on (the first one should also still be on)

When the switch is pressed the third time, both LEDs turn off

Repeat this same cycle of LEDs turning on and off in sequence (off, one LED, two LEDs, off…)



Wiring Diagram:

![wiring diagram](wiring-diagram.png)

Wiring:

![Wiring](wiring.jpg)



Attempt 1: 

Failure. After uploading the code, both small lights lit up. 

This code cannot correctly track the number of button presses and switch between three states. The for loop inside it executes continuously in each iteration of the loop() function, rather than based on the number of button presses.

![01](01.png)

![01_](01_.jpg)



![]()
