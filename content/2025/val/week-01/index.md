---
title: Week 01
date: 2025-10-27T16:58:00.000+02:00
authors:
  - Valerie Jeyeon Yong
image: featured.jpeg
showBgImage: false
---
**Code**

```java
int button;
int i=0;

void setup() {
  // put your setup code here, to run once:
  pinMode(15,OUTPUT); // LED n°1
  pinMode(13,OUTPUT); // LED n°2
  pinMode(16,INPUT); // Button
  Serial.begin(9600); // Sending speed from Pico
}

void loop() {
  // put your main code here, to run repeatedly:
  button = digitalRead(16);
  Serial.println(button);

  if(button == 1){
    i++;

    switch(i){
      case 1: digitalWrite(15,HIGH); break; // Turn on LED n°1 when button is pressed once
      case 2: digitalWrite(13,HIGH); break; // Turn on LED n°2 when button is pressed the second time
      case 3: i = 0; break;                 // Turn off all when button is pressed the third time
    }
  }

  if(i == 0){
    digitalWrite(15,LOW); // Turn off LED n°1
    digitalWrite(13,LOW); // Turn off LED n°2
  }

  delay(100);
}
```

**Pictures of my circuit**

![Pictures of my circuit without light](no-light.jpeg)

![Pictures of my circuit with one light](one-light.jpeg)

![Pictures of my circuit with two light](two-light.jpeg)

**Short description/reflection on my process**

The coding part was easy, so I wrote it quickly. But at first, I set the delay to 10, and that made the button’s info (0 and 1) update too fast. Because of that, pressing the button once produced too many 1. As a result, unless I pressed and released the button extremely quickly, both LEDs stayed on all the time. I didn’t realize it was a delay issue at first, but once I figured it out and changed the delay to 100, it worked fine. (However, if I keep holding the button, I can see the stages changing continuously.) 

Otherwise, the circuit part was simple to make, because I had to add just one LED to another GP (in my case, GP 13).
