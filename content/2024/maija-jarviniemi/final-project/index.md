---
title: Final project
date: 2024-11-11T14:11:00.000Z
authors:
  - Maija Järviniemi
image: featured.jpg
showBgImage: false
---
## **Project proposal guidelines:**

*Create a project proposal for your final project. Think of this as a constantly evolving document where you take notes on your final project ideas.* 

* *Some short thoughts on what you would like to explore and work with conceptually and technically. What is it that you want to make? How does it look like? What does it do?*
* *Some references (artworks/projects that are similar to what you would like to do)*
* *Initial list of parts and components you think you will need*

## **Here we go!**

**Starting point:**

\- I am expanding my project I presented in  Computation A&D course on Fri 8th of Nov, link to [the Miro board](https://miro.com/app/board/uXjVLIm3lwU=/).

\- In **p5.js** I am planning to do **drawing exercises on spirals and other circular paths** inspired by old *special figure skating* figures as well as beautifully illustrated instruction patterns of old ballroom dances

\- **In Physical Computing** I aim to focus on creating **a user interface** that enables interaction with the drawing exercises

![](figureskating1_skateguardblog.jpg)

![](https://www.actingarchives.it/media/showtime/storage/2020/01/08/6/main/fig-4-schema-del-minuetto-in-kellom-tomlinson-the-art-of-dancing-explained-london-1735.jpg?1579507307)

**What kind of interaction and UI?**

OPTION 1: 

* A physical control, possibly a potentiometer functioning as a slider --> A nicely rotating knob that fits nicely to one's hand (my hand ofc), a beautiful to look and use
* Perhaps two knobs for different properties?
* The patterns are drawn to a screen/a projected surface
* Similar knob size I have in mind but it controls the mouse: https://www.instructables.com/Desktop-Scroll-Wheel-and-Volume-Control/

![](https://content.instructables.com/F10/2GP3/FJXP7W7G/F102GP3FJXP7W7G.jpg?auto=webp&frame=1&fit=bounds&md=MjAxMy0xMi0xMCAwNzo1NToxOS4w)

A comment: *This option is the most straigth-forward one with physical knobs and buttons. It would be fun to tinker with different design options for the knobs (how it feels when touched, color, materials, the overall user experience).*

OPTION 2:

* A sensor detects the movement/steps of a user (is there a better word for this...?)
* A projector projecting to the floor where the user is located, translates the sensor data to draw patterns based on my p5.js algorithm
* Perhaps it creates patterns, that the user needs to follow to perform a dance...

A comment: *This is a playful option with the whole body interaction. However this approach adds a new level of complexity for detecting movement and for me to come up with a goooood and algorithm for the p5.js part. I am a quite worried of time.*

OPTION 3:

* A sensor in something wearable detecting movement of the feet for example or hands
* A projector OR a screen that translates the sensor data to draw the images with my algorithm
* Similar project:
  {{<youtube mNd5eXS-0k8>}}

A comment: *This is the hardest option but also quite interesting.  I might get into trouble with time since I am still a beginner in coding + tinkering with sensors and wearables is an unknown terrain for me. It would most likely develop into a too complex project for now.*

**Parts and components needed:**

For OPTION 1: 

* Arduino
* Potentiometer(s)
* Perhaps a button

For OPTION 2: 

* Arduino
* A sensor for detecting movement --> LOOK INTO THIS
* A projector
