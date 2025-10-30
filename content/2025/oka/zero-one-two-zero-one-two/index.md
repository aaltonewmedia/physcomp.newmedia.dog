---
title: Zero, one, two, zero, one, two...
date: 2025-10-30T21:00:00.000+02:00
authors:
  - Oka
image: featured.jpg
bgimage: featured.jpg
showBgImage: true
---
Week 01 assignment, with button presses cycling through no leds on, one led on, two leds on, and back to no leds on.

Made it on my own up to this point:

```
int button;
bool firstLedOn = false;
bool secondLedOn = false;
int buttonState; //idk how to check for a change in the state (''':

void setup() {
  pinMode(15,OUTPUT); //1ST LED
  pinMode(14,OUTPUT); //2ND LED
  pinMode(16,INPUT);
  digitalWrite(15,LOW);
  digitalWrite(14,LOW);
  Serial.begin(9600);
}

void loop() {
  button = digitalRead(16);
  Serial.println(button);

  if(button == 1){
    analogWrite(15,255); //1st led full brightness
    firstLedOn = true;
  }

  if(button == 1 && firstLedOn == true){
    analogWrite(14,255); //2nd led full brightness
    firstLedOn = true;
    secondLedOn = true;
  }

  if(button == 1 && firstLedOn == true && secondLedOn == true){
    analogWrite(15,0); //1st led zero brightness
    analogWrite(14,0); //2nd led zero brightness
    firstLedOn = false;
    secondLedOn = false;
  }
  
  delay(10);
}
```

So the cycle is happening, just at the speed of light: each "1" the button press outputs cycles through all the if statements in the loop(). Couldn't wrap my head around how to track the change between 0 and 1 instead! Might post a fixed update later. (--:
