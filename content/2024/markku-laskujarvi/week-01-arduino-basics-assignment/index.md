---
title: "Week 01: Arduino Basics assignment"
date: 2024-10-27T22:09:00.000Z
authors:
  - Markku Laskujärvi
image: featured.png
showBgImage: false
---
The first actual homework task for this course was challenging for me in many ways. I figured out the initial logic pretty quickly, that I needed a so-called "state machine" to perform the functions described by Matti.

I drafted a rough digital outline (as instructed by Matti) of the functionality and logic in Blender game engine, since im most comfortable in that environment and Python.

Next, I started implementing the idea onto Arduino, which I started by checking out the Arduino documentation: <https://docs.arduino.cc/built-in-examples/digital/StateChangeDetection/>

This was pretty straightforward and I was soon able to make an alternating LED switch with the button. However, I struggled for a long time to figure out how to modify the code to achieve the states 0-1-2 and change between them - I believe this is due to the new kind of syntax Arduino IDE has I had to learn, and am still learning. I had to resort to checking the solution on the course page to push my code further, here it is: 

```
#define LED_2_PIN 10
#define LED_3_PIN 9
#define BUTTON_PIN 4

#define LED_NUMBER 3

byte LEDPinArray[LED_NUMBER] = { LED_2_PIN,
                                 LED_3_PIN };

unsigned long debounceDuration = 50; // millis
unsigned long lastTimeButtonStateChanged = 0;

byte lastButtonState = LOW;

int LEDIndex = 2;

void initAllLEDs()
{
  for (int i = 0; i < LED_NUMBER; i++) {
    pinMode(LEDPinArray[i], OUTPUT);
  }
}

/*void powerOnSelectedLEDOnly(int index)
{
  for (int i = 0; i < LED_NUMBER; i++) {
    if (i == index) {
      digitalWrite(LEDPinArray[i], HIGH);
    }
    if (i != index) {
      digitalWrite(LEDPinArray[i], LOW);
    }
  }
}*/


void updateLEDs(int index)
{
  if (index == 0) {
    // Turn on the first LED only
    digitalWrite(LEDPinArray[0], HIGH);
    digitalWrite(LEDPinArray[1], LOW);
  }
  else if (index == 1) {
    // Keep the first LED on and turn on the second LED
    digitalWrite(LEDPinArray[0], HIGH);
    digitalWrite(LEDPinArray[1], HIGH);
  }
  else if (index == 2) {
    // Turn both LEDs off
    digitalWrite(LEDPinArray[0], LOW);
    digitalWrite(LEDPinArray[1], LOW);
  }
}


void setup()
{
  initAllLEDs();
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  digitalWrite(LEDPinArray[LEDIndex], HIGH);
}

void loop()
{
  unsigned long timeNow = millis();
  if (timeNow - lastTimeButtonStateChanged > debounceDuration) {
      byte buttonState = digitalRead(BUTTON_PIN);
    if (buttonState != lastButtonState) {
      lastTimeButtonStateChanged = timeNow;
      lastButtonState = buttonState;
      if (buttonState == HIGH) {
        updateLEDs(LEDIndex); // button has been released here!
        LEDIndex++;
        if (LEDIndex >= LED_NUMBER) {
          LEDIndex = 0;
        }
        //powerOnSelectedLEDOnly(LEDIndex);
      }
    }
  }
}
```

And an image of my circuitry: 

![The circuitry for state machine exercise for Week 01 of Physical Computing](week01.jpg)
