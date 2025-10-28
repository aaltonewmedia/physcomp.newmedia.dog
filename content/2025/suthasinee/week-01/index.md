---
title: Week 01
date: 2025-10-28T20:20:00.000+02:00
authors:
  - Suthasinee
image: week01.jpg
showBgImage: false
---
**MY CODE:**

int btnState = 0;
int prevBtnState = 0;
int counter = 0;

void setup() {
  // Open the serial port
  Serial.begin(9600);
  // set the button pin to be an input
  pinMode(15, OUTPUT);
  pinMode(16, OUTPUT);
  pinMode(17, INPUT);
}

void loop() {
  // read the button pin
  btnState = digitalRead(17);

  // check if the button state has changed
  if(btnState != prevBtnState && btnState == 1){
    // print the changed state
    Serial.print("Button state changed to: ");
    Serial.println(btnState);
    counter+= 1;
    if(counter >= 6){
      counter = 0;
    }
    Serial.println(counter);
  }

  if(counter == 0){
    analogWrite(15, 0);
    analogWrite(16, 0);
  } if(counter == 1){
    analogWrite(15, 255);
    analogWrite(16, 0);
  }  if(counter == 2){
    analogWrite(15, 0);
    analogWrite(16, 255);
  }if(counter == 3){
    analogWrite(15, 255);
    analogWrite(16, 255);
  }if(counter == 4){
    analogWrite(15, 0);
    analogWrite(16, 0);
  }if(counter == 5){
    analogWrite(15, random(255));
    analogWrite(16, random(255));
  }

  // save the previous button state for the next loop
  prevBtnState = btnState;
  delay(10);

}

**REFLECTION ON MY PROCESS:**

**Hardware part:**

The button didn't work properly at first because i did not understand i have to wire 3.3 volt from the side of the breadboard to the button too (I thought the pin connected from pico to read input would also provide electricity, but nope, rookie mistake!)

**Coding part**

It sounds easier than done. I tried many way of storing the action of button being pressed into a variable and either + or - 1 in each step or then I tried the modulus way (%) but somehow get lost in my thought again and did not arrive to the destination via that route. All of those routes (and many more) can take me there but the final version is the one I can wrap my mind around. It compares if the button state is the same or different than the previous one, if yes, it will add 1 to the counter variable. Then the options of lights (left, right, both, none, blink) depended on how many counters are there already. When it exceed 6, then  it return to 0 and reset the stage.
