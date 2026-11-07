---
title: WEEK 1
date: 2026-10-27T20:46:00Z
showBgImage: false
showComments: true
heroStyle: background
draft: false
---
### CODE.

```
int btnState;
int counter;
int prevbtnState;

void setup() {

  pinMode(15, OUTPUT);
  pinMode(16, OUTPUT);

  pinMode(17, INPUT);
  Serial.begin(9600);
}

void loop() {

  btnState = digitalRead(17);

  //if (btnState != prevbtnState && btnState == HIGH) {

  if (btnState != prevbtnState) {
    if (btnState == HIGH) {

      counter++;

      if (counter > 2) {
        counter = 0;
      }
    }
  }

  prevbtnState = btnState;

  switch (counter) {

    case 0:
      analogWrite(15, 0);  // LED 1 OFF
      analogWrite(16, 0);  // LED 2 OFF
      break;

    case 1:
      analogWrite(15, 255);  // LED 1 ON
      analogWrite(16, 0);    // LED 2 OFF
      break;

    case 2:
      analogWrite(15, 255);  // LED 1 ON
      analogWrite(16, 255);  // LED 2 ON
      break;
  }

  delay(20);
}

```

### DOCUMENTATION

**Reflection.**

- The logic was simple, but yet hard at the same time.
- I tried different solutions as mentioned in the sample examples below.
- I was good at debugging for common errors first, clearing that up before revisiting the logic of the code.
- I didn’t check the final code answers, or consult for final answers and I see this as the foundation for understanding states which is important in various use cases.
- After consulting in class, I understood the differences in the section for reading the INPUT with the if statements, and controlling the OUTPUT to light up the LED with the switch cases.
- I also understood the logic behind why we need the prevbuttonState variable and updating it after each condition check to the new buttonState.

---

**Session 1.**

- Figuring out the circuit.
  - Initially I thought all the components of the circuit had connect with each other but was advised (by Isi and Adrian) that two separate led circuits and the switch can exist as individual circuits, then the code combines everything together.

**Session 2.**

- First pseudocode.
  1. Read a momentary switch being pressed
    1. serial.println // for button.
  2. When the program starts, both LEDs are off
    1. if >>>> button // both off // low
  3. When the switch is pressed once, the first LED turns on
    1. else if >>>> Button === led 1 on
  4. When the switch is pressed the second time, the second LED turns on (the first one should also still be on)
    1. else if >>>> Button == led 1 || led 2
  5. When the switch is pressed the third time, both LEDs turn off
    1. else if >>>> Button == led 1 || led 2 (off)
  6. Repeat this same cycle of LEDs turning on and off in sequence (off, one LED, two LEDs, off…)
- Results
  - Only lit the first bulb.
  - The first bulb did not stay on continuously.

**Session 3.**

- Use of conditional statements combining comparison and logical operators to check button and state together. (e.g: if(button == LOW && state == 1 ))
  1. Advised (by Oskars) to add another variable that stores button values.
  2. Tested using arrays to store the values as a variable named states, but was advised that it was not working as I needed to call the exact number in the array as one way of calling numbers. (e.g. if(button == LOW && state == [1] ))
  3. Consulted ai.aalto to explain errors I wasn’t sure of.
    1. E.g. Compilation error: ISO C++ forbids comparison between pointer and integer [-fpermissive]

**Session 4.**

- Advised (by Matti and the tips tutorials) to add a variable that checks and stores the previous button state number, comparing it with the current button state and previous button state and whether the button is pressed or not. (e.g. if (buttonState == prevbuttonState && button == HIGH))
  - Also to add a calculation that +1 from the previous buttonstate.
- Results.
  1. Only three conditions worked in different scenarios or changing the logical operators between “ ! = ” and == . The code seemed okay but I could be missing the addition part where the loop adds + 1 to the previous buttonstate number.
    1. Both LED’s off.
    2. One LED on.
    3. Both LED on, but only on button click and not continuously.



![](/images/2026/monrhea/mvtghh9x-62vf73x9.jpeg)

