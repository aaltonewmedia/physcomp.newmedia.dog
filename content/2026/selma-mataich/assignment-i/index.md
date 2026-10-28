---
title: "assignment I "
date: 2026-10-28T18:26:00Z
authors:
  - Selma Mataich
showBgImage: true
showComments: false
heroStyle: background
draft: false
---
As for me, this assignment was quite tedious since I have no background in neither programming nor coding in general. A lot of missteps and long hours trying to understand multiple youtube videos, asking for help and trying to decipher the meaning of example solutions on the course page...***I got there in the end!***  

But since I used a lot of different examples to understand the assignment I decided to try out a bit more things with the buttons and LEDs, which you can see below. 

***[Documentation here](https://drive.google.com/drive/folders/1vAIgHYcM8pUaw2o0IBzKLTb0rHRN0wgP?usp=sharing)*** 

++**Code of assignment 1:**++

```cpp
int led1 = 14;
int led2 = 15;
int btn = 16;
int btnState = 0;
int prevbtnState = 0;
int counter = 0; 


void setup()
{
pinMode(14, OUTPUT);
pinMode(15,OUTPUT);
pinMode (16, INPUT);
Serial.begin (9600);
}

void loop() 
{
        btnState = digitalRead(16);
 
if(btnState != prevbtnState) 
{if(btnState == HIGH) {
        counter++;
        if(counter>2){
         counter = 0;
        }
        Serial.println(counter);
}
}


prevbtnState = btnState;


switch  (btnState = counter)
 {
case 0: digitalWrite(14,LOW);
        digitalWrite(15,LOW);
        break;
case 1: digitalWrite(14,HIGH);
        digitalWrite(15,LOW);
        break;
case 2: digitalWrite(14,HIGH);
        digitalWrite(15,HIGH);
        break;


}


delay(10);
}
```

++**Extra experiments:**++

What if instead of two LED pins there are 4 and the light is turning on in different sequences? 

```cpp
int led1 = 14;
int led2 = 15;
int led3 = 13;
int led4 = 12;
int btn = 16;
int btnState = 0;
int prevbtnState = 0;
int counter = 0; 


void setup()
{
pinMode(12, OUTPUT);
pinMode(13,OUTPUT);
pinMode(14, OUTPUT);
pinMode(15,OUTPUT);
pinMode (16, INPUT);
Serial.begin (9600);
}

void loop() 
{
        btnState = digitalRead(16);
 
if(btnState != prevbtnState) 
{if(btnState == HIGH) {
        counter++;
        if(counter>8){
         counter = 0;
        }
        Serial.println(counter);
}
}


prevbtnState = btnState;


switch  (btnState = counter)
 {
case 0: digitalWrite(12,LOW);
        digitalWrite(13,LOW);
        digitalWrite(14,LOW);
        digitalWrite(15,LOW);
        break;
case 1: digitalWrite(12,HIGH);
        digitalWrite(13,LOW);
        digitalWrite(14,LOW);
        digitalWrite(15,LOW);
        break;
case 2: digitalWrite(12,HIGH);
        digitalWrite(13,HIGH);
        digitalWrite(14,LOW);
        digitalWrite(15,LOW);
        break;
case 3: digitalWrite(12,HIGH);
        digitalWrite(13,HIGH);
        digitalWrite(14,HIGH);
        digitalWrite(15,LOW);
        break;
case 4: digitalWrite(12,HIGH);
        digitalWrite(13,HIGH);
        digitalWrite(14,HIGH);
        digitalWrite(15,HIGH);
        break;
case 5: digitalWrite(12,LOW);
        digitalWrite(13,HIGH);
        digitalWrite(14,LOW);
        digitalWrite(15,HIGH);
        break;
case 6: digitalWrite(12,HIGH);
        digitalWrite(13,LOW);
        digitalWrite(14,HIGH);
        digitalWrite(15,LOW);
        break;    
case 7: digitalWrite(12,HIGH);
        digitalWrite(13,HIGH);
        digitalWrite(14,HIGH);
        digitalWrite(15,HIGH);
        break;  
case 8: digitalWrite(12,LOW);
        digitalWrite(13,LOW);
        digitalWrite(14,LOW);
        digitalWrite(15,LOW);
        break;     



}


delay(5);
}
```

What if this sequence happens without a button in a loop?

```cpp
int led1 = 14;
int led2 = 15;
int led3 = 13;
int led4 = 12;
const int N = 4;  
int i = 0;


void setup() {
pinMode(12, OUTPUT);
pinMode(13,OUTPUT);
pinMode(14, OUTPUT);
pinMode(15,OUTPUT);
pinMode (16, INPUT);
Serial.begin (9600);



}
void loop() {
  
for (int i= 0; i < N; i++)

        digitalWrite(12,HIGH);
        delay(500);
        digitalWrite(13,HIGH);
        delay(500);
        digitalWrite(14,HIGH);
        delay(500);
        digitalWrite(15,HIGH);
        delay(500);
        digitalWrite(12,LOW);
        delay(500);
        digitalWrite(13,LOW);
        delay(500);
        digitalWrite(14,LOW);
        delay(500);
        digitalWrite(15,LOW);
        delay(500);
        
     
}


```

