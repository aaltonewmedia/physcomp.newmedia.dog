---
title: "Week 01 "
date: 2026-10-29T12:12:00Z
authors:
  - Arundathi Rajan
showBgImage: false
showComments: true
heroStyle: background
draft: false
---
Code:

```cpp
int ledpin1 = 14;
int ledpin2 = 15;
int btnpin = 13;
int NumberofLed = 2;

int state;

int number = 0;

bool Pressed = false;


void setup() {
  Serial.begin(9600);
  pinMode(btnpin, INPUT);

  pinMode(ledpin1, OUTPUT);
  pinMode(ledpin2, OUTPUT);

  digitalWrite(ledpin1, LOW);
  digitalWrite(ledpin2, LOW);

}

void loop() {
  state = digitalRead(btnpin);

  if (state == LOW && Pressed == false) {  
    number++;
    Pressed = true;
    if 
    (number > NumberofLed) {
      number = 0;
    }
    Serial.println(number);
  } else if (state == HIGH) {              
    Pressed = false;
  }

  if (number == 0) {
  digitalWrite(ledpin1, LOW);
  digitalWrite(ledpin2, LOW);
} else if (number == 1) {
  digitalWrite(ledpin1, HIGH);
  digitalWrite(ledpin2, LOW);
} else if (number == 2) {
  digitalWrite(ledpin1, HIGH);
  digitalWrite(ledpin2, HIGH);
}

delay(80); //added 

}

```

Notes: The code is a bit moody, and only works according to the instructions occasionally. Not exactly sure what needs to be fixed :p    
  
Edit: Adding a delay seems to have fixed the issue. 

![Week 01.jpeg](/images/2026/arundathi-rajan/mvtgnvre-rwjbwgqs.jpeg)

Process: Did refer to the tutorial at one point in time, as I was having trouble with understanding how the LED's would be connected to the Arduino. Sketching out the distribution of power helped, even though it is probably schematically wrong.  

Reflection: Thinking with tools does trigger new ways to look at something rather than using a tool to achieve a particular output. I was able to draw parallels to a spirit of making that was quite popular during undergrad - FAFO (politically correct version is "fool around and find out", but we went with "f*** around and find out"). Encountering "happy accidents" or "unwanted outputs" in the process of experimentation and iteration feels refreshing. 