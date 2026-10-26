---
title: week 01
date: 2026-10-26T22:00:00Z
authors:
  - peijia
showBgImage: false
showComments: true
heroStyle: background
draft: false
---
```
int btn;
int lastBtn=HIGH;
int step=0; 
void setup() {
  pinMode(12,OUTPUT); 
  pinMode(2,OUTPUT);
  pinMode(4,INPUT_PULLUP); 
  Serial.begin(9600);
  digitalWrite(12, LOW);
  digitalWrite(2, LOW);
}

void loop() {
  btn=digitalRead(4);
  Serial.println(btn);
  if(btn==LOW&&lastBtn==HIGH)/*按下且上次为松开*/{
    step=step+1;
    if(step>2){ 
      step=0;
    }
    if(step==0){
      analogWrite(12,0);
      analogWrite(2,0);
  }else if(step==1){
      analogWrite(12,255);
      analogWrite(2,0);
  }else{   //2
      analogWrite(12,255);
      analogWrite(2,255);
    }
}
lastBtn=btn; 
}
```

((will be done on 27/10 