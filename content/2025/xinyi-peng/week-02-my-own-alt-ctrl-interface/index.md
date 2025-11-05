---
title: Week 02 | My Own Alt+Ctrl Interface
date: 2025-11-05T17:21:00.000+02:00
authors:
  - Xinyi Peng
image: week02_bg.jpg
bgimage: week02_bg.jpg
showBgImage: true
---
# Part I: Find an interesting existing Alt+Ctrl Interface

## Looks like Music by Yuri Suzuki

### Art Description

The artist features robots that are programmed to follow a black line drawn on white paper. They each respond with specific sounds as they pass over coloured marks laid down across the track by visitors.
The devices, called Colour Chasers, are each designed with different shapes and translate the colours they encounter into sounds including drums, deep bass, chords and melody.
Suzuki is gyslexic and cannot read musical scores, but he has a passion to play and create new music and always dream to create new notation of music. So he made this for people like him with no knowledge about musci making to discover the new method to create music.

![](sound_like_music.png)

*see the video and artist potfolio at: <https://yurisuzuki.com/artist/looks-like-music>*

### Imagine How he did that

* The movement of the robot: follow the black lines

  > * Components: RGB sensor + IR LED or IR Sensor Matrix
  > * My guess: From Matti, we know that this kind of sensor works by the reflection of light, so I guess there is an LED on the buttom of the robot, or he just use IR sensors to detect whether there is strong light reflected back(white) or all eaten by the surface(black). If the RGB sensors/IR sensors detect the color is black, then just keep walking straightly; if white, then it will tell the arduino board to turn a bit left/right, keeping tracking the black lines.
* Detect the colors and make sound

  > * Componets: RGB sensor(can be the APDS9960 we test) + IR LED + sound module
  > * My guess: Keep sending the light and recognize the color it goes through, and different color will triger differen sound record from the sound module.

### My Question

* How manny RGB sensors/IR sensor used in this to insure the movement of the robot smoothly?
* Idealy, people draw each color seperately so it can be clearly detected. But what about all the colors are mixed and overlap each other? Does this also works? Or it use the oily pen to avoid two color overlapped together and become sth else?

# Part II: Come Up with A Concept for Your Qwn Alt+Ctrl Interface

## Inspiration: Sensing Kirigami & Paper Machine &

Thoes are two of my favorite tangible interaction programs, and they all use special conductive paper as sensor to create interesting interaction, the **carbon-coated paper** and **paper printed with conductive or thermosensitive ink**. So instead of using the digital components, I am interested in using the material itself as sensor by utilizing its conductivity and resistance.

![](jpg1.jpg)

![](jpg2.jpg)

![](jpg3.jpg)

And then, I am surfing in my brain what game I am fond of, and how can they connect with the conductive paper, and its way of interaction like folding. Then Monument Valley comes into my mind. It uses optical illusion and geometry to create interesting game experience may align with the paper folding. 

![](mv.jpg)

## How Does this works

The core is to map physical behaviors such as "paper folding" with digital feedback such as "virtual space deformation", establishing a perceptible input system and a visual output system. Players aim to interact with the tangible paper interface to send the character to the gate to pass and collect coins. Maybe some storytelling can be added into this just like waht Monument Valley did.

* Hardware: carbon-coated paper, arduino/Raspberry Pi Pico
* Software: processing / unity
* Analog Mapping:

  > Physical action: for instance, fold one paper from 180° to 90°;
  > AnalogRead: value may change from 800 to 300;
  > Scene mapping: a plate fold from 180° to 90°
  > Question: the bending angle can be detected by data, but how does the software knows at which point of the plate it stars to bend? The scene mapping logic I come up with may not work in complex folding situcation.
* Simple game sketch:

  ![](sketch_1.jpg)

  ![](sketch_2.jpg)

  ![](sketch_3.jpg)
