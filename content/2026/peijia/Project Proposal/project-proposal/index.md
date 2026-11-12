---
title: Project Proposal
date: 2026-11-12T14:08:00Z
showBgImage: true
showComments: true
heroStyle: background
draft: false
---
# Website with a Physical Presence

*Making a website respond to human presence*

**Project Idea**

I want to create a website that can sense when someone is nearby and react to their presence.

For example, when nobody is near the computer, the website will be in a sleeping state. The screen will be dark, the animations will be slow, and the lights will be dim.

When someone gets closer, the website will slowly wake up. The shapes will grow, the colours will become brighter, and the animations will become more active.

When someone is very close to the screen, the website will become fully active. The lights behind the screen will also become brighter.

**How it works**

I plan to use Arduino with an ultrasonic distance sensor to detect how close a person is to the computer.

Arduino will send the distance data to the computer. Then JavaScript and p5.js will use this data to change the website's animations and appearance.

I also want to connect an RGB LED strip to Arduino and place it behind the computer screen. The lights will change together with the website.

**Technology and Materials**

- Arduino Uno
- HC-SR04 ultrasonic distance sensor
- RGB LED strip
- Breadboard and jumper wires
- JavaScript
- p5.js

