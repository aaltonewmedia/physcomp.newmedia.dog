---
title: assignment-1
date: 2026-10-28T12:19:00Z
showBgImage: false
showComments: true
heroStyle: background
draft: false
image: /images/2026/lia-sapurova/mvsiqtle-dyqufhew.jpg
---
I broke the exercise into smaller steps, to make sure that I understand each one and only seek support for exact aspects I don't understand.



Step 1. Let there be light

Simply connected 1 LED to the microcontroller, wrote simple simple code that just making sure that it lights up and I can control whether the light is on or off.

Then added the second LED and made sure it was all ok and nothing exploded.



Step 2. Let there be light (sometimes)

Connected the button and adjusted the code to included. Had a confusion at this point, but turned out that I just didn't connect the wire in the right place and my code was correct (oops). 

```
// basic on/off
  if (click == LOW) {
    // turn on the led
    digitalWrite(led1,HIGH);
    digitalWrite(led2,HIGH);
  } else {
    // turn off the led
    digitalWrite(led1,LOW);
    digitalWrite(led2,LOW);
  }
```

![](/images/2026/lia-sapurova/mvsiiay5-g2ktb92j.jpg)

Step 3. One, two, three light

Tried figuring out how to do the challenge part of the exercise. It was kinda interesting because I understood the logic of how to make it work, but didn't know what is the syntax I would use for it. I gathered that I somehow need to record button click in a loop of 3, off/off-off/on-on/on, replacing (or adding to) the boolean logic of an if/else statement with something that can go 3 ways.

At this point I did open the guides Matti provided, but I tried to take it small and reference them small bits at a time, making sure that I understand each step. I found that the syntax I was looking for was "switch", so I sketched an if/else statement and then compared it later with the reference, adjusting it to fit.

Interestingly, for my code to work I needed to claim  (click != LOW)  and not (click != HIGH), because otherwise they would kind of start from one and not from zero. I am not sure why, but if it works it works :D

```
int led1 = 15;
int led2 = 14;
int button = 16;
int click;
int count;
int preclick;

void setup() {
  // put your setup code here, to run once:
  // output; leds
  pinMode(led1,OUTPUT);
  pinMode(led2,OUTPUT);
  // input; button
  pinMode(button,INPUT);

}

void loop() {
  // put your main code here, to run repeatedly:
  // what's up with the button
  click = digitalRead(button);

  if (click != preclick){
    if (click == LOW) {
      count++;

      if(count>2){
        count = 0;
      }
    }
    preclick = click;
  }

  switch (count) {
    case 0:
    digitalWrite(led1,LOW);
    digitalWrite(led2, LOW);
    break;

    case 1:
    digitalWrite(led1,HIGH);
    digitalWrite(led2, LOW);
    break;

    case 2:
    digitalWrite(led1,HIGH);
    digitalWrite(led2, HIGH);
    break;
  }

/* basic on/off
  if (click == LOW) {
    // turn on the led
    digitalWrite(led1,HIGH);
    digitalWrite(led2,HIGH);
  } else {
    // turn off the led
    digitalWrite(led1,LOW);
    digitalWrite(led2,LOW);
  }
*/

  // polite delay
  delay(5);
}

```

There is light!

![](/images/2026/lia-sapurova/mvsikqnz-sde3gt35.jpg)

