---
title: "Final project: The controller"
date: 2024-12-04T20:05:00.000Z
authors:
  - Maija Järviniemi
image: featured.jpg
showBgImage: false
---
[Link to the Miro board](<>)

![](featured.jpg)

## Basic concept: 

My initial inspiration was the sensation of drawing with finger on sand. Hence, I wanted to focus on the touch and how it feels on one's finger to draw slowly a spiral. The optimal controller would have a soft furrow that does not have any sharp edges. One important factor for me is also the beauty of the controller so that it would invite you to touch it.

### 19.11.2024 Things to consider

Here's a sensor that might work with this kind of spiral slider: [https://learn.bela.io/products/trill/about-trill/](<>)

* Matti told that the spiral could be cut from **copper sticker in Fablab** -> they have a suitable material for the vinyl cutter
* **3d model** for the top part but also for the wiring etc beneath:

  * Rounded edge for the spiral for soft touch
  * What's the size of the controller? 
  * The thickness/depth of the "furrow" (ura in Finnish)?
  * Is there some kind of structure inside for the wiring? How are the top and bottom part attached? 
  * A 3d printed bottom layer for the copper sticker to put on, is it just a square/circle or a spiral as well? To ease the wiring?
* For the spiral:

  * How much margin/saumavara for the wires if they are soldered on the sides of the spiral
  * **In how many sections should the spiral be cut?** 30 was the number in one the Bela sensors
  * How to fit the spiral perfectly with the 3d printed top part? 
* For now I think the controller will be connected to the computer with usb-c cable where it will also get power from -> in the future version it would need batteries
* Figure out how to **connect the data** from the sensor with **p5.js**

### The Trill Ring sensor

For the sensor to work, it needed two pull-up resistors between SCL + 5 v and SDA + 5v. Prior the custom spiral sensor, I am going to use a Trill ring sensor with the same slider capacities.

[Here's a link to the instructions from Matti](https://learn.adafruit.com/working-with-i2c-devices/pull-up-resistors)

![](trill-ring-resistors.jpg)

The working palette with sensor, resistors etc. 

![](trill-ring.jpg)

**THE ARDUINO AND SENSOR VALUES**

The sensor detects multiple touches, the location and the size of the touch. The two values below are ***location*** and ***size.*** The values for the location are between 0 and 3500(+100) while sliding the ring clockwise. 

![](monitordatatrillring-web.png)

![](plotterdatatrillring-web.png)

*How to transfer the data to p5.js --> [https://learn.newmedia.dog/courses/physical-computing/week-04/lesson-01/](<>)*

[](https://learn.newmedia.dog/courses/physical-computing/week-04/lesson-01/)Here's a code from p5.js to connect the arduino data to draw a circle. Remember to add the following line inside the **head** tags in the **index.html** :

```html
<script src="https://unpkg.com/@gohai/p5.webserial@^1/libraries/p5.webserial.js"></script>
```

```javascript
//Example from course website
//https://learn.newmedia.dog/courses/physical-computing/week-04/lesson-01/


let port;
let c;
let s=10;
let touch;

function setup() {
  createCanvas(600, 600);
  port = createSerial(); //connect to arduino
  c = color(255);
}

function draw() {
  background(255, 250, 207);
  
  if(port.available()>0){
  touch = port.readUntil("\n");
    port.clear();
    c = map(touch,0,3550,0,255,true); //0-3590 values from arduino
    s = map(touch,0,3550,10,400);
  }
  fill(255,0,0);
  text("touch: " + touch,20,40);

  fill(c);
  circle(width/2, height/2, s);
}

function mousePressed(){
  if (!port.opened()) {
    port.open(115200);
  }
}
```

[](https://learn.newmedia.dog/courses/physical-computing/week-04/lesson-01/)

{{<vimeo 1031182690>}}

### 24.11.2024 3d print samples

* Sampling with 3d printer the width and depth of the furrow, with the help of Miro
* Size of the spiral - I coded the spiral with p5.js :)
* Best furrow measurements were 2mm (depth), 120mm (full width), 60mm (open area/hole)

![](img_2019web.jpg)



### Mon 25.11 - Sun 1.12.2024

* Along the week drawing and fixing vector files for the copper tape + the laser cut pieces from acrylic -> a support structure for the spiral and buttons
* Finished the spiral and the copper tape pieces --> done by hand with a 3d printed "stencil" -> took quite many tries to get it right :D
* A prototype printed for the cover -> Miro helped with the model, figuring out the support structure and 3d printing

![](physcomp_process-web_041224.jpg)

![](physcomp_process-web_041224-2.jpg)

### Mon 2.12.2024

* Soldering of the spiral sensors and buttons to a bunch of wires (22 pieces) --> looks like a shrimp!
* Figuring out the trill sensor position on the case --> adding a piece of felt/Eva foam by sewing and attaching that with 2-sided tape on to laser cutted layer

![](physcomp_process-web_041224-3.jpg)

### Tue 3.12.2024

* 3d print for the case cover (the bottom part not ready yet) --> turned out to be quite bad quality from Aalto 3d print workshop so I decided to go with the earlier version 
* Soldering the trill craft sensor to the copper pieces + breadboard...  -> this took many hours of figuring out the placing of wires so that they don't touch each other -> this prevents the capacitive sensors taking some unwanted signals when close to each other
* I used the pins 0-18, 19 sensors in total for the spiral slider

![](physcomp_process-web_041224-4.jpg)

### Wed 4.12.2024

* Final soldering of the button wires, pin 25 for the left button sensor and pin 29 for the right button sensor
* Attached the Trill Craft to a piece of Eva foam and that to the acrylic layer with 2-sided tape
* Attached the SCL, SDA, power (5V), grounding via breadboard and 2 resistors to Arduino board
* Tested with Matti and it works! Also succeeded in connecting the slider to my p5.j sketches -> still needs some adjusting with lerping and the visuals to get the calm and smooth animation I want
* 3d printed a bottom piece for the controller to protect the wiring -> not the best solution but works for now. Arduino and breadboard are still separate from the controller. Miro made the 3d model for the bottom cover with my instructions

![](physcomp_process-web_041224-5.jpg)

### The final piece (shown in 5.12.2024)

**Possible improvements for the controller:**

* Covering the copper with a fully 3d printed cover?
* Smaller copper tape piece for the buttons -> I made them too big so now they also react to touch when touching the cover next to them
* Smaller arduino/raspberry pi and somehow making the resistor wiring more tight perhaps without the breadboard?
* Rethinking the aesthetics of the cover and the shape -> more organic texture and material. Could the shape be circular? To me the shape and material feeling is too "clinical"

**Comments about the p5.js and code:**

* Instead of altering the angle of the spirals controlling the number of  "points" so that the spiral builds itself while following the spiral path with finger -> would possibly provide more satisfying experience for the user :D
* More advanced spirals paths etc -> will continue with this to figure out the logic and math!

**Other:**

I enjoyed most of the craftwork and material experience related to this project:

* Making the spiral slider and attaching the copper tape pieces to it. Piece by piece it reminded me of an ancient jewelry due to the copper and the scratch marks.
* Soldering was fun and at some point the ancient jewelry slider turned into a glimmering shrimp! 
* Figuring out the wiring from the slider to the Trill craft sensor and then soldering. Though it felt almost impossible task to keep the wires deattached from each other, I was extremely happy I managed to pull it of :) 

First I had hard time of coming up with a physical computing concept that would work with my p5.js project. So once the idea came, I started working with it straight away. This project was my very first take on working with electronics so it really put me on my toes. For the last minute I was expecting something to fail. Hence the surprise of it working without any major errors really drew a big smile on my face! 

\- something about other ideas and future
