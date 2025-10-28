---
title: Week 1 Assignment
date: 2025-10-28T23:49:00.000+02:00
authors:
  - Mikael Malin
image: assignment.png
showBgImage: false
---
**Starting point**

I started from the circuit we made in class: the led turns on when the button is pressed down. It was a good starting point since I had a working circuit that I could compare to if something went wrong.

**Version 1**

My first goal was to modify the circuit so that once you press the button, the light stays on without keeping it pressed down, and turns off when pressed again. It had been a while since I wrote any code, but I referred to the tutorials on Learn New Media site.

* I wrote the code to check if the button state changes, and if the change is rising or falling by comparing to the previous button state.
* I added variables for the led state and previous led state (1 = on, 0 = off).
* Then I wrote an if statement: 
* * If the change of the button state is rising (button is pressed) and the led is off, the led turns on — if the led is on it turns off.
  * If the change of the button state is falling (button is released) and the led is on, it stays on — if the led is off, it stays off. 

**Bugs:**

* Otherwise works as intended, but sometimes the light flickers really fast on and back off though it should stay on.

**Version 2**

For the next version I made a circuit with two leds and a button, where both turn on when the button is pressed and off when pressed again. I did it this way to first see if I had managed to connect the new led correctly.

* Bugs: still the same flickering issue from time to time, decided to proceed to the next version and ask help later when there’s tutoring available

**Version 3**

Next I kept the previous statements, but added these if statements:

* For a rising change (button pressed)
* If the red led is off, it turns on
* If the red is on and the green is off, the green turns on
* If both the red and the green are on, both turn off.
* For a falling change (button released), keep the previous values.

**Debugging**

* Matti helped me solve the flickering issue: Pico’s loops are too fast for my human fingers so sometimes it jumps to the next stage too quickly. I solved this with adding a delay as advised for a quick fix, and also learned a more sophisticated way to fix it is called debouncing.

**Endgame**

Seeing the correct solution, I understand my code may have been a bit inefficient, I should have used a counter. However it works as intended and I’m ok with the result.



```
int buttonState = 0;
int ledState = 0;
int prevButtonState = 0;
int prevLedState = 0;
int buttonPin = 16;
int ledPin = 15;


void setup() {
 Serial.begin(9600);
 pinMode(buttonPin, INPUT);
 pinMode(ledPin, OUTPUT);
 }


void loop() {
 buttonState = digitalRead(buttonPin);
 if (buttonState != prevButtonState) {
   //if the button changes state from LOW to HIGH (finger on button):
   if (buttonState == HIGH){
     if (prevLedState == 0) {
     ledState = 1;    
     }else{
     ledState=0;
     }
   }
   //if the button changes state from HIGH to LOW (finger off button):
   if (buttonState == LOW){
     if (prevLedState == 1){
       ledState = 1;
     }else{
     digitalWrite(ledPin, LOW);
     ledState=0;
     }
   }
 }
 if (ledState == 1){
     digitalWrite(ledPin, HIGH);
 } else {
   digitalWrite(ledPin, LOW);
 }
 //save the state for the next loop.
 prevLedState = ledState;
 prevButtonState = buttonState;
}
```
