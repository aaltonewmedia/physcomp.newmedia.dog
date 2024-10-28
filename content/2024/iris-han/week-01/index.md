---
title: WEEK 01
date: 2024-10-28T22:11:00.000Z
authors:
  - Iris Han
image: feature.jpg
bgimage: 544fa16ad4cbfe45ffbe1556111cdc43.jpg
showBgImage: false
---


![](week1_1.jpg)

![](week1_2.jpg)

```
int led1=9;
int led2=6;
int btnVal=0;
int btnNewstate;
int btnLaststate=LOW;

void setup() {
  pinMode(led1,OUTPUT);
  pinMode(led2,OUTPUT);
  pinMode(2,INPUT);
  Serial.begin(9600);
}

void loop() {
 btnNewstate = digitalRead(2);
 
 Serial.println(led1,led2);

 if(btnNewstate==HIGH && btnLaststate == LOW){
  btnVal++;
 }

 btnLaststate = btnNewstate;

 if(btnVal==1){
   digitalWrite(9,HIGH);
 }else if(btnVal==2){
   digitalWrite(9,HIGH);
   digitalWrite(6,HIGH);
 }else if(btnVal==3){
   digitalWrite(9,LOW);
   digitalWrite(6,LOW);
   btnVal=0;
 }
 delay(10);
}
```

Reflection:

I struggled for a long time in "button state" part. I was so confused about how to write button pressed for 3 times appropriately. And I finally found out that I can seperate two states of button and make the first begin button state as LOW. When I first pressed the button, "btnNewstate==HIGH && btnLaststate == LOW" is true, then btnVal became 1. Then the condition"btnVal==1" can be executed. This homework help me understand how Arduino's code and circuit interact and relate well.
