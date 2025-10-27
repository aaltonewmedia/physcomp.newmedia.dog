---
title: Week 01
date: 2025-10-27T11:46:00.000+02:00
authors:
  - Milla Heinisuo
image: featured.jpg
showBgImage: false
---
placeholder image :D
## Circuit

\[photo here]

## Code

```
int button;
int prevButton;
int i;
bool isPressed;

void setup() {
  // put your setup code here, to run once:
  pinMode(15, OUTPUT);
  pinMode(13, OUTPUT);
  pinMode(16, INPUT);
  digitalWrite(15, LOW);
  digitalWrite(13, LOW);
  Serial.begin(9600);

  i = 0;
}

void loop() {

  button = digitalRead(16);

  if (button == HIGH && prevButton == LOW && !isPressed) {
    i++;
    isPressed = true;
    delay(20);
  }

  if (button == LOW && prevButton == HIGH) {
    isPressed = false;
    delay(20);
  }

  if (i == 1) {
    digitalWrite(15, HIGH);
    digitalWrite(13, LOW);
  } else if (i == 2) {
    digitalWrite(15, HIGH);
    digitalWrite(13, HIGH);
  } else if (i == 3) {
    digitalWrite(15, LOW);
    digitalWrite(13, LOW);
    i = 0;
  }

  prevButton = button;
}
```

## Reflection

...
