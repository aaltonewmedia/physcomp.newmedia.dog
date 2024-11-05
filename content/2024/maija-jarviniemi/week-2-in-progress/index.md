---
title: Week 2 IN PROGRESS
date: 2024-11-01T20:20:00.000Z
authors:
  - Maija Järviniemi
image: featured.jpg
showBgImage: false
---
![]()

## 1 Alt+CTRL interface

I chose *Ghostboi* as an interesting Alt+ctrl interface. It is a game, where you control a ghost character with a bendy pink controller. The controller molded from translucent silicone in the shape of a cartoon ghost works with touch sensors and vibration motors giving haptic feedback for the player. The storyline deals with emotions and memories of mortal life that the ghostboi character faces during his journey to afterlife.

{{<youtube kD_QNWwgXTA>}}

While browsing the projects, the pink color and cute aesthetics made me open the link (so obvious). It was nice to see how the actual experience of touching an interface, its materials, shape and color, were part of the storyline of the game. I don't really play video games, but the theme of nostalgia, afterlife and mortality also felt fresh in this context. 

Link to the project: <https://shakethatbutton.com/ghostboi-in-the-land-of-the-dead/>

## 2. Come up with a concept for your own Alt+Ctrl Interface

* *Explore at least one sensor in more detail + figure out how to read the values from it with your Arduino. Print out the data from the sensor to the Serial Monitor or Serial Plotter.*
* *Find out what type of data you can get out of it. Is it on/off signal, continuous number, some more complex data?*
* *Think of different interactions and/or gestures that could be detected with the sensor you picked. What kind of objects could the sensor be attached to?*
* *Choose an existing video game that could be controlled using the interaction/interface enabled by this sensor OR come up with a completely new game or game mechanic.*
* *Come up with the idea/concept for your alternative controller. These can be silly, experimental, ridiculous, absurd…*

## Let's start!

**What I chose to work with:**

1. **Video game**: Crash Bandicoot 1, level 8: Hog Wild
2. **Sensor:** MSA301 triple-axis accelerometer
3. **Tool/object for interaction** - a yo-yo, controls the hog's jump (y-axis) and movement to left + right (x- and z-axis)

![](yoyo-interaction.jpg)

*Source for the edited illustration of a yo-yo: James L. Haven; Charles Hettrick - US patent application 59745, James L. Haven; Charles Hettrick, "Whirligig", published 1866-11-20. Public domain.*



In Arduino, the sensor gives 3 numeral values with decimals, both - and + , but I'm not sure about the range. First value is x, second y, and third z.

![](accelerometer_monitor.png)

![](accelerometer_plotter.png)

{{<youtube X2yDxSFadHM>}}
