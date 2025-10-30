---
title: Week1
date: 2025-10-30T13:21:00.000+02:00
authors:
  - Elias Ruokanen
image: photo_2025-10-30_13-46-13.jpg
bgimage: ""
showBgImage: false
---
Most of the week I spent reading up on and watching videos of electric circuits. It's always been kind of a blind spot for me. The direction of current, the role of ground, all of that was just very unintuitive to me. It took a while to understand.



Once I had my head somewhat wrapped around circuits I thought it would be easy. The coding is not a problem, and if I just hook up the wires it should just work... Nope.



One led won't light up. Is the wire not connected? Is it not getting power? Try a different pin on the arduino. Why does that work? Why doesn't the button work? Why does it sometimes work? I copied the code from the example to rule out problems. Still doesn't work. Try different wires, different pins, different resistors, wiggle things around. Is the system in state 0, 1 or 2? You won´t get power from pin 9 if it´s in the wrong state. Tear my hear out. Turns out one led was broken.



This was a learning experience. What I learned is that I need a systematized approach when I build a circuit. The connections can be very finicky. Parts may be broken. Make sure every part works on its own. Otherwise I spend the night troubleshooting. Seems kinda obvious in retrospect.
