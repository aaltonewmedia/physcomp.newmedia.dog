---
title: Week_1 Homework
date: 2024-10-28T00:02:00.000Z
authors:
  - Connor Z
image: feature.jpg.jpg
bgimage: background.jpg
showBgImage: true
---
**First Arduino Work
INSTRUCTION**
Create a circuit and Arduino code that does the following

**Circuit #**
Connect two LEDs to your Arduino using a breadboard
Connect one switch to your Arduino using a breadboard
**Code #**
Read a momentary switch being pressed
When the program starts, both LEDs are off
When the switch is pressed once, the first LED turns on
When the switch is pressed the second time, the second LED turns on (the first one should also still be on)
When the switch is pressed the third time, both LEDs turn off
Repeat this same cycle of LEDs turning on and off in sequence (off, one LED, two LEDs, off…)

Circuit

![2 LED and 1 BUTTON](w1.png)

Connecting the second LED is not hard, basically I just copied how LED 1 is connected. 

Firse LED - Connected to Port 4

Second LED - Connected to Port 8

![Actual Attempt](whole.jpg)

And it works. 

I tested it out by activating both LED when I pressed the button. All are connected. 

(Except the first time I put the wrong port on code and confused about why it doesn't work)



**CODE PART**

**First Attempt**

The logic to control the LEDs turn on and off looks like this 

```
int btnPin = 2;
int btnVal = 0; 
bool LED_4, LED_8; //set bool
int btnPreVal = false; 

if(btnVal == 1){

    //turn on first LED 
    digitalWrite(4, HIGH); 
    LED_4 = true; 
    
  }
  else if(btnVal == 1 && LED_4 == true){
    //turn on the sesond LEd
    digitalWrite(8, HIGH); 
    LED_8 = true; 

  }
  else if (btnVal == 1 && LED_4 == true && LED_8 == true){
    digitalWrite(4, LOW); 
    digitalWrite(8, LOW); 
    LED_4 = false; 
    LED_8 = false; 
  }
```

It doesn't work. 

Only the LED_4 will light up and then nothing will change after this. 

And then, I figured that under this condition, only the code under the first if statement will run. Once the condition is fulltilled, the rest of the code will be ignored. 



Second Attempt

I tried to fix the logic. 

```
if(btnVal == 1 && btnPreVal == false){

    if(!LED_4 && !LED_8){

    //turn on first LED 
    digitalWrite(4, HIGH); 
    LED_4 = true; 
    //digitalWrite(8, LOW); 
    }
    else if(LED_4 && ! LED_8){
    //turn on the sesond LEd
    digitalWrite(8, HIGH); 
    LED_8 = true; 

    }
    else if (LED_4 && LED_8){
    digitalWrite(4, LOW); 
    digitalWrite(8, LOW); 
    LED_4 = false; 
    LED_8 = false; 

    }

  }


  delay(10); 
  btnPreVal = btnVal;
```

I write the condition under an if statement to make sure the button presse can be read. 

After it enters the if statement, my code will say: "okay, let's check which condition we fullfilled! ". 

1. if both LED are not on, turn on the first LED

2. if the first one is on but second one it not, turn on the second LED

3. if both LED are on, turn of both. 

And everytime we get out from the loop, make sure that the btnPreVal is set equals to btnVal (which should be 0/false).

AND IT WORKS. 

 

Both LEDs are OFF

![](1.jpg)



Press the button and the first LED is turn on

![](2.jpg)



Press the button again. The first LED remains on, and the second LED is turned on

![](3.jpg)



Press the button, both LEDs are turned off

![](4.jpg)



Thank  you :)

(I know you like my feature pictures and my bg pictures, I will put my favourite characters for all the homework posts. )







Week 1

Soryu Asuka Langley and her EVA 02 

Evagelion
