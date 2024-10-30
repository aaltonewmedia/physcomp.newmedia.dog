---
title: "Assignment 01: LED Counter"
date: 2024-10-30T17:12:00.000Z
authors:
  - Fiona Keil
image: featured.jpg
bgimage: ""
showBgImage: true
---


**Assignment:**



Create a circuit and Arduino code that does the following



Circuit #

\- Connect two LEDs to your Arduino using a breadboard

\- Connect one switch to your Arduino using a breadboard



Code #

1. Read a momentary switch being pressed

2. When the program starts, both LEDs are off

3. When the switch is pressed once, the first LED turns on

4. When the switch is pressed the second time, the second LED turns on (the first one should also still be on)

5. When the switch is pressed the third time, both LEDs turn off

6. Repeat this same cycle of LEDs turning on and off in sequence (off, one LED, two LEDs, off…)



**My Solution:**

I started by altering the breadboard from the single-light switch that we made last class. I pretty much just moved the switch further away and plugged in a second light with it's own pin connection. See the attached breadboard picture for what I ended up with.

I was struggling with how to make it so there is 3 different states in the code. I tried to use the if else code how we always did in p5.js but it was hard to figure out how to actually do it because I only remember doing it with two states.

I ended up making an int called 'press' which tracks how often the button was pressed and trying to make the button state depend on that.



I ended with this situation:\

The first light successfully turns on on the first press, but the second one doesn't for some reason. The first light remains turned on on the second press as it should and then turns off on the third press as it is supposed to, so the counter at least works. \

\\
\
Breadboard:\
Code:

**Attempted fixes:\**

\- Debugging but got no errors. \

\- Using a different light and wires on my breadboard \

\--> unfortunately neither worked, so there must be some error in my structure or code which I was unable to identify!\

\--> I tried making changes in my code and in wiring but just managed to break what I got working so I returned it to the 'best state' I managed to achieve.

\--> I also tried asking AI to help find points where there could be problems but it didn't give any useful feedback besides what I already tried.

\
**Thoughts:**

In a nutshell, it was frustrating. It feels like the task is easy and the way to connect the wires is straightforward and when it doesn't work it is hard to fix it due to the electric component. It is much easier to fix code I think. I will probably need to work more in the mechatronics workshop to get some help for my final project of this is already too much for me. On the bright side I feel encouraged already to get an Arduino for myself to use at home because I understand how to set it up better. Hopefully after this course I am good enough to do some things with it by myself or at least know where to look for solutions in a more efficient way.
