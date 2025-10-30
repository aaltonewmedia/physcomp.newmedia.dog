---
title: "week 01: two lamps"
date: 2025-10-29T14:02:00.000+02:00
authors:
  - Klara Bloch-Norup
image: 01.jpg
showBgImage: false
---
```
int ledPin1 = 14;
int ledPin2 = 15;
int button;
int currentState;
int prevState;
int count;

void setup() {
  // put your setup code here, to run once:
  pinMode(ledPin1, OUTPUT);  //green
  pinMode(ledPin2, OUTPUT);  //orange
  pinMode(16, INPUT);        //button

  Serial.begin(9600);

  //both lamps off
  digitalWrite(ledPin1, LOW);
  digitalWrite(ledPin2, LOW);
}

void loop() {

  //button pressed = one on
  //button pressed: count = count + 1
  //count is 2 = both is on
  //count is 3 = both is off

  button = digitalRead(16);
  //Serial.println(button);

  currentState = button;

  //check if current state is different from previous
  if (currentState != prevState) {
    if (currentState == HIGH) {
      //add one to the count
      count++;
    }
    //check if both lamps are on and make the count zero
    if (count > 2) {
      count = 0;
    }
    Serial.println(count);
  }

  //store the state for next loop
  prevState = currentState;


  /*
  if (count == 0) {
    digitalWrite(ledPin1, LOW);
    digitalWrite(ledPin2, LOW);
  }

  if (count == 1) {
    digitalWrite(ledPin1, HIGH);
    digitalWrite(ledPin2, LOW);
  }

  if (count == 2) {
    digitalWrite(ledPin1, HIGH);
    digitalWrite(ledPin2, HIGH);
  }
  */

  switch (count) {
    case 0:
      digitalWrite(ledPin1, LOW);
      digitalWrite(ledPin2, LOW);
      break;
    case 1:
      digitalWrite(ledPin1, HIGH);
      digitalWrite(ledPin2, LOW);
      break;
    case 2:
      digitalWrite(ledPin1, HIGH);
      digitalWrite(ledPin2, HIGH);
      break;
  }


  //add a small delay, so you dont catch small uncertanties just before the button gets contact, this is another way to do the auduino debouncing technique
  delay(10);
}


```
