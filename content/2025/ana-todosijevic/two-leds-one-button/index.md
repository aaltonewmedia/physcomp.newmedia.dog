---
title: "Two LEDs one Button "
date: 2025-10-30T12:00:00.000+02:00
authors:
  - Ana Todosijevic
image: photo_2025-10-30_12-03-08.jpg
bgimage: photo_2025-10-30_12-03-12.jpg
showBgImage: true
---
I didn't solve it completely on my own, I had to peek at the solution. The main reason I was struggling was that I didn't really think through how the button works - I thought I needed to reset it to 0, and I spent too much time trying to do that. The other reason was that I kept LED instructions (which one is on and which one is off) in the same loop as the counter, which just ended up keeping both LEDs on.

```
int buttonState;
int prevState;
int counter;

void setup() {
  // put your setup code here, to run once:
  pinMode(15,OUTPUT);
  pinMode(13,OUTPUT);
  pinMode(16,INPUT);
  digitalWrite(15,LOW);
  digitalWrite(13,LOW);
  Serial.begin(9600);
  counter = 0;
  prevState = 0;
}

void loop() {
  // put your main code here, to run repeatedly:
  buttonState = digitalRead(16);
  Serial.println(buttonState);
  if(buttonState != prevState){
    if(buttonState==1){
      counter++;
      if(counter>2){
        counter=0;
      }
    }
  }
  prevState = buttonState;
  if(counter == 0){
    digitalWrite(13,LOW);
    digitalWrite(15,LOW);
  }
  if(counter == 1){
    digitalWrite(13,HIGH);
    digitalWrite(15,LOW);
  }
  if(counter == 2){
    digitalWrite(13,HIGH);
    digitalWrite(15,HIGH);
  }

  delay(10);
}
```
