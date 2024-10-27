---
title: Week 1
date: 2024-10-28T00:19:00.000Z
authors:
  - xinchen.l
image: img_0670.jpg
showBgImage: false
---
* Here's a version that failed, feels like a connection problem

![](wechatimg384.jpg)

int led1 = 8;          

int led2 = 5;          

int button = 2;        



int pressCount = 0;     // googled

void setup() {

  

  pinMode(led1, OUTPUT);  

  pinMode(led2, OUTPUT);



  

  pinMode(button, INPUT);



  // led off

  digitalWrite(led1, LOW);  

  digitalWrite(led2, LOW);

}



void loop() {

  // read the button

  int buttonState = digitalRead(button);



  

  if (buttonState == HIGH) {



\    delay(200);          // don’t know if delay is necessary 



\    pressCount = pressCount + 1;

\    // googled how to read press twice



\    

\    if (pressCount == 1) {

\    digitalWrite(led1, HIGH);   

\    } 

\    else if (pressCount == 2) {

\    digitalWrite(led2, HIGH);   

\    } 

\    else if (pressCount == 3) {

\    digitalWrite(led1, LOW);     

\    digitalWrite(led2, LOW);

\    pressCount = 0;              

\    }

\    

   

  }

}
