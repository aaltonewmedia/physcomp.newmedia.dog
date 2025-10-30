---
title: "LED 1 2 3 "
date: 2025-10-29T22:34:00.000+02:00
authors:
  - Heu
image: img_3700.jpeg
showBgImage: false
---
The hardware part was fun, and coding was not fun. 

It took hours and hours (+ a few hints) to figure out. I misunderstood button state (only on or off) and counter (1, 2, 3). After I set up the counter, the numbers was adding up, but when I tried to reset the counter to 1 2 3 1 2 3, I accidentally broke the code. Somehow my Command Z didn't work, so that was another few hours gone. Finally, I found out I had to use two "==" instead of one... 

After a tiny break from coding, I need to clarify when to use the following: 

\- digitalWrite and analogWrite

\- difference between else and else if

Apologies for my simple language, after consecutive days of coding, I do not have the brain power to write properly... 

```
int button;
int buttonState = 0;
int prevButtonState = 0;
int counter = 0;

void setup() {
  // put your setup code here, to run once:
  pinMode(14, OUTPUT);
  pinMode(15, OUTPUT);
  pinMode(16, INPUT);
  digitalWrite(15, LOW);  //sending 3.3v. high means turning it on
  digitalWrite(14, LOW);
  Serial.begin(9600);  //open up a communication port between pico and computer
}

void loop() {
  // put your main code here, to run repeatedly:
  buttonState = digitalRead(16);  //reading a value from the pin.
  //println(button);this will have an error, pico does not have anything to print to
  //digitalWrite(100,HIGH);

  if (buttonState != prevButtonState) {
    Serial.print("Button state: ");
    Serial.println(buttonState);

     if (buttonState == HIGH) {
      //Serial.print("Count:");
      Serial.println(counter);
      counter++;
      Serial.print("Count:");
      Serial.println(counter);
    }
    if (counter == 1){
      analogWrite(14, 255);
    }else if (counter == 2){
      analogWrite(15, 255);
    }else if (counter == 3){
      analogWrite(14, 0);
      analogWrite(15, 0);
    }else if (counter == 4){
      counter = 1;
    }
    }
    prevButtonState = buttonState;
  delay(10);
  }
```
