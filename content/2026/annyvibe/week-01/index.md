---
title: WEEK 01
date: 2026-10-27T16:48:00Z
image: /images/2026/annyvibe/mvqt3zlf-4u4nah5k.jpg
showBgImage: false
showComments: true
heroStyle: background
draft: false
---
## **Assignment 01: LED Counter**



The task is to connect two LEDs and one push button to an Arduino on a breadboard, then write a program that counts button presses: both LEDs start off, the first press turns on the first LED, the second press turns on the second LED as well, and the third press turns both off, repeating the cycle of off → one LED → two LEDs → off.  


###   
**Diagram**

![](/images/2026/annyvibe/week01/mvqt7b1j-k463jbx8.jpg)



### **Circuit**

![](/images/2026/annyvibe/week01/mvqt7dug-ypwf5s2o.jpg)



### **Code**

```
int button;
int lastButton = LOW;   
int count = 0;          

void setup() {
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, INPUT);
  Serial.begin(9600);
}

void loop() {
  button = digitalRead(8);

  
  if (button == HIGH && lastButton == LOW) {
    count = count + 1;
    if (count > 3) {
      count = 1;        
    }

    switch (count) {
      case 1:
        digitalWrite(6, HIGH);
        digitalWrite(7, LOW);
        break;
      case 2:
        digitalWrite(6, HIGH);
        digitalWrite(7, HIGH);
        break;
      case 3:
        digitalWrite(6, LOW);
        digitalWrite(7, LOW);
        break;
    }
    Serial.print("Press times:");
    Serial.println(count);
  }

  lastButton = button;
  delay(50);          
}
```

