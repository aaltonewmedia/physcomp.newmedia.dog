---
title: final project
date: 2025-11-03T18:55:00.000+02:00
authors:
  - Klara Bloch-Norup
image: website_documentation_1_01.png
showBgImage: false
---
## KNITTED SURFACES

in collaboration with textile designer, Lila , I will work with knitted fabrics as the interface

we are working with conductive yarn and wool as the main materials and we might combine machine knit with hand knit

CREATING SOUND

we are interested in sound as the feedback of the interaction with a knitted textile 

LIGHT COULD BE ADDED TOO

I am curious about implementing light as well

and I have made the prototype of a knitted lamp, that might be interesting to include in the setting – if it works out together with the knitted interface and in the physical setting of this project

open for inputs, ideas and collaborations, please send an email to klara.bloch-norup@aalto.fi 

![](physicalcomputing_a5_02-03.png)

![](physicalcomputing_a5_04-05.png)

![](physicalcomputing_a5_06-07.png)

![](physicalcomputing_a5_08-09.png)

![](physicalcomputing_a5_12-13.png)

![](physicalcomputing_a5_14-15.png)

![](physicalcomputing_a5_16-17.png)

![](physicalcomputing_a5_18-19.png)

The Arduino code:

```
/*********************************************************
This is a library for the MPR121 12-channel Capacitive touch sensor

Designed specifically to work with the MPR121 Breakout in the Adafruit shop 
  ----> https://www.adafruit.com/products/

These sensors use I2C communicate, at least 2 pins are required 
to interface

this sketch demonstrates the auto-config chip-funktionality.
for more information have a look at
https://github.com/adafruit/Adafruit_MPR121/issues/39
https://github.com/adafruit/Adafruit_MPR121/pull/43


based on MPR121test
modified & extended by Carter Nelson/caternuson

MIT license, all text above must be included in any redistribution
**********************************************************/
#include <Wire.h>
#include <Adafruit_MPR121.h>
#include "Keyboard.h"
#ifndef _BV
#define _BV(bit) (1 << (bit)) 
#endif

Adafruit_MPR121 cap = Adafruit_MPR121();

// Keeps track of the last pins touched
// so we know when buttons are 'released'
uint16_t lasttouched = 0;
uint16_t currtouched = 0;

void dump_regs() {
  Serial.println("========================================");
  Serial.println("CHAN 00 01 02 03 04 05 06 07 08 09 10 11");
  Serial.println("     -- -- -- -- -- -- -- -- -- -- -- --"); 
  // CDC
  Serial.print("CDC: ");
  for (int chan=0; chan<12; chan++) {
    uint8_t reg = cap.readRegister8(0x5F+chan);
    if (reg < 10) Serial.print(" ");
    Serial.print(reg);
    Serial.print(" ");
  }
  Serial.println();
  // CDT
  Serial.print("CDT: ");
  for (int chan=0; chan<6; chan++) {
    uint8_t reg = cap.readRegister8(0x6C+chan);
    uint8_t cdtx = reg & 0b111;
    uint8_t cdty = (reg >> 4) & 0b111;
    if (cdtx < 10) Serial.print(" ");
    Serial.print(cdtx);
    Serial.print(" ");
    if (cdty < 10) Serial.print(" ");
    Serial.print(cdty);
    Serial.print(" ");
  }
  Serial.println();
  Serial.println("========================================");
}

void setup() {
  delay(1000);
  Serial.begin(115200);
  Keyboard.begin();

  //while (!Serial);
  Serial.println("MPR121 Autoconfiguration Test. (MPR121-AutoConfig.ino)");
  
  Serial.println("startup `Wire`");
  Wire.begin();
  Serial.println("startup `Wire` done.");
  delay(100);
  
  Serial.println("cap.begin..");
  if (!cap.begin(0x5A, &Wire)) {
    Serial.println("MPR121 not found, check wiring?");
    while (1);
  }
  Serial.println("MPR121 found!");
  
  delay(100);

  Serial.println("Initial CDC/CDT values:");
  dump_regs();

  cap.setAutoconfig(true);

  Serial.println("After autoconfig CDC/CDT values:");
  dump_regs();
}

void loop() {
  // Get the currently touched pads
  currtouched = cap.touched();
  
  for (uint8_t i=0; i<12; i++) {
    // it if *is* touched and *wasnt* touched before, alert!
    if ((currtouched & _BV(i)) && !(lasttouched & _BV(i)) ) {
      Serial.print(i); Serial.println(" touched");
      Keyboard.press(i+48);
    }
    // if it *was* touched and now *isnt*, alert!
    if (!(currtouched & _BV(i)) && (lasttouched & _BV(i)) ) {
      Serial.print(i); Serial.println(" released");
      Keyboard.release(i+48);
    }
  }

  // reset our state
  lasttouched = currtouched;
}
```

VS Code – drawing 3 dynamic shapes on canvas:

```
//Global variables
let x;
let xa;
let xb;
let o = 100; //opacity could be dynamic at some point
let fft;
let spectrum;
let treble;
let highMid;
let mid;
let lowMid;
let bass;
let bird, sea, glass05, glass06, branches, moreBranches, drum, wind, underWater, ceramic, piano, reeds, whiteNoise, synth, Synth_4, Synth_9;
let audioFilter; //low pass filter for sound

function preload() {
  bird = loadSound("data/bird.m4a");
  glass05 = loadSound("data/hitting_glass_05.WAV");
  glass06 = loadSound("data/hitting_glass_06.WAV");
  branches = loadSound("data/branches.WAV");
  moreBranches = loadSound("data/more_branches.WAV");
  drum = loadSound("data/Steel tongue drum.m4a");
  wind = loadSound("data/Strong wind_ZOOM0001_MN.WAV");
  sea = loadSound("data/suomelinna sea.m4a");
  underWater = loadSound("data/under_water.wav");
  ceramic = loadSound("data/Humm and beep at ceramic WS.m4a");
  piano = loadSound("data/Piano chord.m4a");
  reeds = loadSound("data/Reeds.WAV");
  whiteNoise = loadSound("data/White noise_ZOOM0002_MN.WAV");
  synth = loadSound("data/synth audio_eg.wav");
  Synth_4 = loadSound("data/cmaj4_synth.wav");
  Synth_9 = loadSound("data/Cmaj9_synth.wav");
}


function setup() {
  createCanvas(windowWidth, windowHeight);
  //smoothing value and how many bins added in the parantheses
  //specific steps: 1024 is max: 16, 32 ...
  fft = new p5.FFT(0.8, 32);
  whiteNoise.play(); 
  whiteNoise.setLoop(true);
  whiteNoise.setVolume(0.8);
  audioFilter = new p5.LowPass(); // not really used yet
}


//Function to play sounds with key 0-9
//CLICK ON THE SCREEN TO MAKE IT WORK
function keyPressed() {
    if (key === "1") {
      bird.play();
    } else if (key === "2") {
      glass05.play();
    } else if (key === "3") {
      glass06.play();
    } else if (key === "4") {
      branches.play();
    } else if (key === "5") {
      ceramic.play();
    } else if (key === "6") {
      sea.play();
    } else if (key === "7") {
      wind.play();
    } else if (key === "8") {
      synth.play();
    } else if (key === "9") {
      piano.play();
    } else if (key === "0") {
      drum.play();
    }
    
    //PRESS SPACE TO STOP ALL SOUNDS
    if (keyCode === 32){
      bird.stop();
      glass05.stop();
      glass06.stop();
      branches.stop();
      //moreBranches.stop();
      drum.stop();
      wind.stop();
      sea.stop();
      //underWater.stop();
      ceramic.stop();
      piano.stop();
      //reeds.stop();
      //whiteNoise.stop();
      synth.stop();
      //Synth_4.stop();
      //Synth_9.stop();
    }
}


function draw() {
  background(200, 220, 240);
  noStroke();
  spectrum = fft.analyze();
  let v = map(mouseY, height, 0, 0, 1);
  sea.setVolume(v);


  //SHAPE A-B-01, CENTER
  //TRANSLATE – to position center the shape
  let shapeX = width / 2;
  let shapeY = 0;
  push();
  fill(0, 0, 0);
  translate(shapeX, shapeY);
  fill(0, 0, 0, o); //opacity added
  //variables for SHAPE A
  x = 80;
  xb = x - 80;
  ya = height / 14;
  yb = height / 35;
  y = (height - (10 * ya + 8 * yb)) / 2;
  //SHAPE A
  beginShape();
  vertex(x, 0);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        treble = fft.getEnergy("treble");
        let s = map(treble, 0, 255, 120, 600);
        xa = x + s;
      }
      if (i == 2) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 120, 600);
        xa = x + s;
      }

      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 120, 600);
        xa = x + s;
      }

      if (i == 6) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 120, 600);
        xa = x + s;
      }

      if (i == 8) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 120, 600);
        xa = x + s;
      }

      bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
      y = y + ya * 2;
    } else {
      bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
      y = y + yb * 2;
    }
  }
  vertex(x, height);
  //updating variables so they are suitable for left side of shape
  x = x - 160;
  xa = x - s;
  xb = x + 80;
  vertex(x, height);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 2) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 6) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 8) {
        treble = fft.getEnergy("treble");
        s = map(treble, 0, 255, 120, 600);
        xa = x - s; }
      bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
      y = y - ya * 2;
    } else {
      bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
      y = y - yb * 2;
    }
  }
  vertex(x, 0);
  endShape(CLOSE);


  //ANOTHER SAME SHAPE
  fill(0, 0, 0);
  //variables for SHAPE B
  x = 80;
  xb = x - 80;
  ya = height / 14;
  yb = height / 35;
  y = (height - (10 * ya + 8 * yb)) / 2;
  //SHAPE B
  beginShape();
  vertex(x, 0);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        treble = fft.getEnergy("treble");
        let s = map(treble, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 2) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 6) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 8) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 80, 400);
        xa = x + s; }
      bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
      y = y + ya * 2;
    } else {
      bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
      y = y + yb * 2; }
  }
  vertex(x, height);
  //updating variables so they are suitable for left side of shape
  x = x - 160;
  xa = x - s;
  xb = x + 80;
  //left side of SHAPE B
  vertex(x, height);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 80, 400);
        xa = x - s; }
      if (i == 2) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 6) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 8) {
        treble = fft.getEnergy("treble");
        s = map(treble, 0, 255, 80, 400);
        xa = x - s;
      }
      bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
      y = y - ya * 2;
    } else {
      bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
      y = y - yb * 2; }
  }
  vertex(x, 0);
  endShape(CLOSE);
  pop();




  //SHAPE A-B-02, LEFT
  //TRANSLATE – to position center the shape
  shapeX = width/4;
  shapeY = 0;
  push();
  fill(0, 0, 0);
  translate(shapeX, shapeY);
  fill(0, 0, 0, o); //opacity added
  //variables for SHAPE A
  x = 80;
  xb = x - 80;
  ya = height / 14;
  yb = height / 35;
  y = (height - (10 * ya + 8 * yb)) / 2;
  //SHAPE A
  beginShape();
  vertex(x, 0);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        treble = fft.getEnergy("treble");
        let s = map(treble, 0, 255, 120, 600);
        xa = x + s;
      }
      if (i == 2) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 120, 600);
        xa = x + s;
      }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 120, 600);
        xa = x + s;
      }
      if (i == 6) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 120, 600);
        xa = x + s;
      }
      if (i == 8) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 120, 600);
        xa = x + s;
      }
      bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
      y = y + ya * 2;
    } else {
      bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
      y = y + yb * 2;
    }
  }
  vertex(x, height);
  //updating variables so they are suitable for left side of shape
  x = x - 160;
  xa = x - s;
  xb = x + 80;
  vertex(x, height);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 2) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 6) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 120, 600);
        xa = x - s; }
      if (i == 8) {
        treble = fft.getEnergy("treble");
        s = map(treble, 0, 255, 120, 600);
        xa = x - s; }
      bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
      y = y - ya * 2;
    } else {
      bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
      y = y - yb * 2;
    }
  }
  vertex(x, 0);
  endShape(CLOSE);

  //SHAPE B
  fill(0, 0, 0);
  //variables for SHAPE B
  x = 80;
  xb = x - 80;
  ya = height / 14;
  yb = height / 35;
  y = (height - (10 * ya + 8 * yb)) / 2;
  //SHAPE B
  beginShape();
  vertex(x, 0);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        treble = fft.getEnergy("treble");
        let s = map(treble, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 2) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 6) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 80, 400);
        xa = x + s; }
      if (i == 8) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 80, 400);
        xa = x + s; }
      bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
      y = y + ya * 2;
    } else {
      bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
      y = y + yb * 2; }
  }
  vertex(x, height);
  //updating variables so they are suitable for left side of shape
  x = x - 160;
  xa = x - s;
  xb = x + 80;
  //left side of SHAPE B
  vertex(x, height);
  vertex(x, y);
  for (let i = 0; i < 9; i++) {
    if (i % 2 == 0) {
      if (i == 0) {
        bass = fft.getEnergy("bass");
        s = map(bass, 0, 255, 80, 400);
        xa = x - s; }
      if (i == 2) {
        lowMid = fft.getEnergy("lowMid");
        s = map(lowMid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 4) {
        mid = fft.getEnergy("mid");
        s = map(mid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 6) {
        highMid = fft.getEnergy("highMid");
        s = map(highMid, 0, 255, 80, 400);
        xa = x - s;
      }
      if (i == 8) {
        treble = fft.getEnergy("treble");
        s = map(treble, 0, 255, 80, 400);
        xa = x - s;
      }
      bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
      y = y - ya * 2;
    } else {
      bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
      y = y - yb * 2; }
  }
  vertex(x, 0);
  endShape(CLOSE);
  pop();





   //SHAPE A-B-03, RIGHT
   push();
   shapeX = width/4*3;
   shapeY = 0;
  
   fill(0, 0, 0);
   translate(shapeX, shapeY);
   fill(0, 0, 0, o); //opacity added
   //variables for SHAPE A
   x = 80;
   xb = x - 80;
   ya = height / 14;
   yb = height / 35;
   y = (height - (10 * ya + 8 * yb)) / 2;
   //SHAPE A
   beginShape();
   vertex(x, 0);
   vertex(x, y);
   for (let i = 0; i < 9; i++) {
     if (i % 2 == 0) {
       if (i == 0) {
         treble = fft.getEnergy("treble");
         let s = map(treble, 0, 255, 120, 600);
         xa = x + s;
       }
       if (i == 2) {
         highMid = fft.getEnergy("highMid");
         s = map(highMid, 0, 255, 120, 600);
         xa = x + s;
       }
       if (i == 4) {
         mid = fft.getEnergy("mid");
         s = map(mid, 0, 255, 120, 600);
         xa = x + s;
       }
       if (i == 6) {
         lowMid = fft.getEnergy("lowMid");
         s = map(lowMid, 0, 255, 120, 600);
         xa = x + s;
       }
       if (i == 8) {
         bass = fft.getEnergy("bass");
         s = map(bass, 0, 255, 120, 600);
         xa = x + s;
       }
       bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
       y = y + ya * 2;
     } else {
       bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
       y = y + yb * 2;
     }
   }
   vertex(x, height);
   //updating variables so they are suitable for left side of shape
   x = x - 160;
   xa = x - s;
   xb = x + 80;
   vertex(x, height);
   vertex(x, y);
   for (let i = 0; i < 9; i++) {
     if (i % 2 == 0) {
       if (i == 0) {
         bass = fft.getEnergy("bass");
         s = map(bass, 0, 255, 120, 600);
         xa = x - s; }
       if (i == 2) {
         lowMid = fft.getEnergy("lowMid");
         s = map(lowMid, 0, 255, 120, 600);
         xa = x - s; }
       if (i == 4) {
         mid = fft.getEnergy("mid");
         s = map(mid, 0, 255, 120, 600);
         xa = x - s; }
       if (i == 6) {
         highMid = fft.getEnergy("highMid");
         s = map(highMid, 0, 255, 120, 600);
         xa = x - s; }
       if (i == 8) {
         treble = fft.getEnergy("treble");
         s = map(treble, 0, 255, 120, 600);
         xa = x - s; }
       bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
       y = y - ya * 2;
     } else {
       bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
       y = y - yb * 2;
     }
   }
   vertex(x, 0);
   endShape(CLOSE);
 
   //SHAPE B
   fill(0, 0, 0);
   //variables for SHAPE B
   x = 80;
   xb = x - 80;
   ya = height / 14;
   yb = height / 35;
   y = (height - (10 * ya + 8 * yb)) / 2;
   //SHAPE B
   beginShape();
   vertex(x, 0);
   vertex(x, y);
   for (let i = 0; i < 9; i++) {
     if (i % 2 == 0) {
       if (i == 0) {
         treble = fft.getEnergy("treble");
         let s = map(treble, 0, 255, 80, 400);
         xa = x + s; }
       if (i == 2) {
         highMid = fft.getEnergy("highMid");
         s = map(highMid, 0, 255, 80, 400);
         xa = x + s; }
       if (i == 4) {
         mid = fft.getEnergy("mid");
         s = map(mid, 0, 255, 80, 400);
         xa = x + s; }
       if (i == 6) {
         lowMid = fft.getEnergy("lowMid");
         s = map(lowMid, 0, 255, 80, 400);
         xa = x + s; }
       if (i == 8) {
         bass = fft.getEnergy("bass");
         s = map(bass, 0, 255, 80, 400);
         xa = x + s; }
       bezierVertex(x, y, xa, y + ya, x, y + ya * 2);
       y = y + ya * 2;
     } else {
       bezierVertex(x, y, xb, y + yb, x, y + yb * 2);
       y = y + yb * 2; }
   }
   vertex(x, height);
   //updating variables so they are suitable for left side of shape
   x = x - 160;
   xa = x - s;
   xb = x + 80;
   //left side of SHAPE B
   vertex(x, height);
   vertex(x, y);
   for (let i = 0; i < 9; i++) {
     if (i % 2 == 0) {
       if (i == 0) {
         bass = fft.getEnergy("bass");
         s = map(bass, 0, 255, 80, 400);
         xa = x - s; }
       if (i == 2) {
         lowMid = fft.getEnergy("lowMid");
         s = map(lowMid, 0, 255, 80, 400);
         xa = x - s;
       }
       if (i == 4) {
         mid = fft.getEnergy("mid");
         s = map(mid, 0, 255, 80, 400);
         xa = x - s;
       }
       if (i == 6) {
         highMid = fft.getEnergy("highMid");
         s = map(highMid, 0, 255, 80, 400);
         xa = x - s;
       }
       if (i == 8) {
         treble = fft.getEnergy("treble");
         s = map(treble, 0, 255, 80, 400);
         xa = x - s;
       }
       bezierVertex(x, y, xa, y - ya, x, y - ya * 2);
       y = y - ya * 2;
     } else {
       bezierVertex(x, y, xb, y - yb, x, y - yb * 2);
       y = y - yb * 2; }
   }
   vertex(x, 0);
   endShape(CLOSE);
   pop();
}
```



The Arduino code for the light:

```
#include <Adafruit_NeoPixel.h>

// Which pin on the Arduino is connected to the NeoPixels?
#define LED_PIN 2

// How many NeoPixels are attached to the Arduino?
#define LED_COUNT 105

// NeoPixel brightness, 0 (min) to 255 (max)
#define BRIGHTNESS 100  // set BRIGHTNESS to about 1/5 (max = 255)

// Declare our NeoPixel strip object:
Adafruit_NeoPixel pixels(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);
// Argument 1 = Number of pixels in NeoPixel strip
// Argument 2 = Arduino pin number (most are valid)
// Argument 3 = Pixel type flags, add together as needed:
//   NEO_KHZ800  800 KHz bitstream (most NeoPixel products w/WS2812 LEDs)
//   NEO_KHZ400  400 KHz (classic 'v1' (not v2) FLORA pixels, WS2811 drivers)
//   NEO_GRB     Pixels are wired for GRB bitstream (most NeoPixel products)
//   NEO_RGB     Pixels are wired for RGB bitstream (v1 FLORA pixels, not v2)
//   NEO_RGBW    Pixels are wired for RGBW bitstream (NeoPixel RGBW products)


/*
//TEST
//SOUND VARIABLES
int treble = 255;
int highMid = 200;
int mid = 150;
int lowMid = 100;
int bass = 50;
*/


void setup() {
  pixels.begin();  // INITIALIZE NeoPixel strip object (REQUIRED)
  pixels.show();   // Turn OFF all pixels ASAP
  pixels.setBrightness(BRIGHTNESS);
  Serial.begin(9600);
}

void loop() {

  // if there's any serial available, read it:
  while (Serial.available() > 0) {

    // look for the next valid integer in the incoming serial stream:
    int treble = Serial.parseInt();
    // do it again:
    int highMid = Serial.parseInt();
    // do it again:
    int mid = Serial.parseInt();
    // do it again:
    int lowMid = Serial.parseInt();
    // do it again:
    int bass = Serial.parseInt();


    // look for the new line. That's the end of your sentence:
    if (Serial.read() == '\n') {

      //SET PIXEL COLORS IN LED STRIP
      for (int i = 0; i < 21; i++) {
        pixels.setPixelColor(i, pixels.Color(treble, treble, treble));
      }

      for (int i = 21; i < 42; i++) {
        pixels.setPixelColor(i, pixels.Color(highMid, highMid, highMid));
      }

      for (int i = 42; i < 63; i++) {
        pixels.setPixelColor(i, pixels.Color(mid, mid, mid));
      }

      for (int i = 63; i < 84; i++) {
        pixels.setPixelColor(i, pixels.Color(lowMid, lowMid, lowMid));
      }

      for (int i = 84; i < 105; i++) {
        pixels.setPixelColor(i, pixels.Color(bass, bass, bass));
      }

      pixels.show();
      delay(1);

      Serial.print(treble);
      Serial.print(",");
      Serial.print(highMid);
      Serial.print(",");
      Serial.print(mid);
      Serial.print(",");
      Serial.print(lowMid);
      Serial.print(",");
      Serial.println(bass);
    }
  }
}
```
