---
title: WEEK1
date: 2026-10-27T22:40:00Z
authors:
  - Jin
showBgImage: true
showComments: true
heroStyle: background
draft: false
bgimage: mvr5kkbm-ovyq4fl2.jpg
image: /images/2026/jin/mvr5kkbm-ovyq4fl2.jpg
---
// Sketch

![](/images/2026/jin/mvr5p84q-yzgtbxrh.jpg)

// Connecting everything

![cee6308eba624cd322b63201b9cf2e8.jpg](/images/2026/jin/mvr641n8-6eg11hfa.jpg)

// Coding

```
int state = 0;
int lastButton = HIGH;
int nowButton;

void setup() {
 pinMode(20, OUTPUT);
 pinMode(21, OUTPUT);
 pinMode(19, INPUT_PULLUP);
 Serial.begin(9600);
}

void loop() {
nowButton = digitalRead(19);

 if (nowButton == LOW && lastButton == HIGH) {
  state = state + 1;
 if (state == 3) {
  state = 0;
 }

 delay(50);
 }

 lastButton = nowButton;

 if (state == 0) {
 digitalWrite(20, LOW);
 digitalWrite(21, LOW);
 }

 if (state == 1) {
 digitalWrite(20, HIGH);
 digitalWrite(21, LOW);
 }

 if (state == 2) {
  digitalWrite(20, HIGH);
  digitalWrite(21, HIGH);
 }
}
```

// And then… nothing happened

Checked the wiring (seemed right), then checked the code. Still nothing... well, thanks to Anny (credit to her xddd) finally figured out what went wrong. Turns out, I’d connected the wires to the edges of the switch AGAIN (literally the SECOND time I’ve made the same stupid mistake…)

*// Didn’t get to the “Read a momentary switch being pressed” part yet, the whole switch situation had already drained my battery for the day.*

*-- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --*