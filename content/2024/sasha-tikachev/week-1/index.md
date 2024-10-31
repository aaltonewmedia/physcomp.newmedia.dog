---
title: week 1
date: 2024-10-31T11:15:00.000Z
authors:
  - Sasha Tikachev
image: screenshot-2024-10-31-at-11.11.31.png
showBgImage: false
---
# Assignment 1

## demonstration

![works](https://nofuture.lol/uino.gif)

## Code with comments (debouncing approach):

```cpp
int ledPin1 = 10;
int ledPin2 = 9;
int buttonPin = 3;
int buttonRead;
int buttonState;            // the current reading from the input pin
int lastButtonState = LOW;  // the previous reading from the input pin
int ledCount = 0;

/* debouncing is done as in the tutorial: https://docs.arduino.cc/built-in-examples/digital/Debounce/ */

/* unsigned for handling large numbers, since using milliseconds */

unsigned long lastTimer = 0;  // the last time the button was pressed/released
unsigned long debounceDelay = 50;    // the debounce time; 

void setup() {
 pinMode(ledPin1, OUTPUT);
 pinMode(ledPin2, OUTPUT);
 pinMode(buttonPin, INPUT);
 Serial.begin(9600);
}


void loop() {
  buttonRead = digitalRead(buttonPin);
 Serial.println(ledCount);
//listening the button state
 if (buttonRead != lastButtonState) {
   //starting to track the time after the button was pressed or released
   lastTimer = millis();
 }
 if ((millis() - lastTimer) > debounceDelay) { // If sufficient time has passed since last bttn state change
   if (buttonRead != buttonState) { //and button state changed again
     buttonState = buttonRead;
    if (buttonState == HIGH) { //updating the count value (0-2)
     ledCount++;
     if (ledCount==3) 
        ledCount=0;
    }}}
   if (ledCount == 0) { //writing the led states depending on the current count for each cycle of the loop
       digitalWrite(ledPin1, LOW);
       digitalWrite(ledPin2, LOW);
       } else if (ledCount==1)
         {
           digitalWrite(ledPin1, HIGH);
           digitalWrite(ledPin2, LOW);
         } else {
             digitalWrite(ledPin1, HIGH);
             digitalWrite(ledPin2, HIGH);
         }
 lastButtonState = buttonRead;
}

```
