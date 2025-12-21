---
title: Final Project Process & Reflection
date: 2025-12-21T20:10:00.000+01:00
authors:
  - Heu
image: featured.jpeg
showBgImage: false
---
**Process Documentation / Notes**

**Week 1 Summary:**

I had ideas about micro sound, secret message, and started testing ideas immediately. We connected Arduino to MIDI through Ableton, programmed Morse code and electromagnet with pico, and connected Max to pico through serial port.

**Detailed Notes:** 

251113

* Connected Polulu A4988 Black Edition with one motor  

  <https://www.pololu.com/product/2128>
* Connected the stepper motor with MIDI via Ableton, using the Physical Pixel code example. While sending MIDI notes to step motor, I had an idea about using secret code as an interpretation of non-verbal sonic narration.
* Reference about micro sounds Nelo Akamatsu's Chijikinkutsu - A spatial composition without using speakers, with needles inside glasses. The signals were sent through copper wires with an iPad using sequencer notes.
* Researched about secret codes - started to use Morse code (dots and dashes).

Issue: the stepper motor got too hot\
→ Adjusting the trimpot (resistance) with screw driver and using multi meter to measure

Motor maximum current: 1.4A\
Over heating → changing it to run at 0.7A\
RCS (sense resistor) = 0.068Ω

Calculation:\
VREF = 8 x IMAX x RCS\
VREF = 8 x 0.7A x 0.068 ohms (driver RCS) = 0.38 volts

Steps: \
Power the stepper motor to measure voltage (not 5v from arduino)\
Set multimeter to measure DC voltage\
Multimeter black probe to ground\
Multimeter red probe to metal screw of the trimpot\
Turn the trimpot until VREF reads 0.38 volts (380mV)\
This gives motor 0.7A peak current and prevent from overheating

**Week 2 Summary:** 

Began experimenting with electromagnets and controlling them with Pico and Max, and explored Morse code as a temporary system before further developing the composition.

**Detailed Notes:** 

251117

* Experimented with one electromagnet and paper clips directly with power source without connecting with pico.
* Decided to use electromagnet instead of solenoids, because solenoid itself generates more noise. Electromagnet is silent, which is suitable for micro sounds.

251118

* Connected electromagnet with pico following the diagram from the below website using TIP120 transistor and a button to control on/off independently (very proud)

  <https://deepbluembedded.com/arduino-electromagnet-control-circuit-code-example/>
* Programmed "hello" in Morse code in arduino with a tediously long code...
* Connected Max with Arduino → sending on/off using Physical Pixel (Ascii chart H-72 L-76)
  many progress in one afternoon!

Notes:

* With one pico - I can connect with 20-30 magnets. using ascii chart and use other letters to send.

Issue: 

When the magnet releases the paper clip still sticks on the electromagnet due to the remaining magnet → the hanger would need to pull back a bit or with carefully adjusted distance.

251120

* Composition with Morse Code → how does it sound like?
* Used text to Morse Code online translator and downloaded five audio files from it. Then, I created five channels in Reaper and IEM plug-in to hear it in binaural format to hear how it sounds like when five of them are played at the same time.
* Connected one motor with one electromagnet - one dash and one dot (with Matti's help)

```
#define EM_PIN 19

#define STEP_PIN 6
#define DIR_PIN 7

void setup() {
  //step pin
  pinMode(6,OUTPUT);
  //direction pin
  pinMode(7,OUTPUT);

  //magnet 1
  pinMode(19,OUTPUT);
  //magnet 2 
  pinMode(18,OUTPUT);

  //digitalWrite(21,LOW);
  Serial.begin(9600);
}

void loop() {
   
   dot();
   delay(100);
   dot();
   delay(100);
   dot();
   delay(100);
   dash();
   delay(100);
   dash();
   delay(100);
   dash();
   delay(100);
   dot();
   delay(100);
   dot();
   delay(100);
   dot();
   delay(100);
   delay(1000);
  }

  void dot(){
   digitalWrite(19,HIGH);
   digitalWrite(18,HIGH);
   delay(50);
   digitalWrite(19,LOW);
   digitalWrite(18,LOW);
   delay(50);
  }

  void dash(){
    for (int i = 0; i < 400; i++){
      oneStep();
      delayMicroseconds(2000);
    }
  }

void oneStep() {
  digitalWrite(STEP_PIN, HIGH);
  delayMicroseconds(1);
  digitalWrite(STEP_PIN, LOW);
  delayMicroseconds(100);
}
```

Creating two functions that defines 'dot' and 'dash'. Using for loop to ask the motor to spin for a certain amount of time / steps. Add a timing without delays.

If it's a dot, do a dot.
Delay 
If it's a dash, do a dash.
Delay
Step to the next index and start again. 

* Talked about my idea with Oliver, and concluded not using Morse Code for the library project. If I am not using Morse code in the end, do I need to spend all my time on the technical and compositional side with Morse code.
* Re-setting goals for the prototype

**Week 3 Summary:**

Progress stopped due to Computational Art...

251125

* connected with two electromagnets - playing the same thing

Instead of serial port, connected to Max via MIDI.

Motor → Channel 1\
Magnets → Channel 2
Magnet 1 → note 60
Magnet 2 → note 61

New goal: creating two working systems

* Max for performance
* Pico for installation

Rather than programming a long code to tell the motor to stop and start, working with MIDI is the most flexible → why?

Array of notes\
Array of note length \
A note or a pause\
Some sort of tempo and step through the notes

**Week 4 Summary:**

Made a generative system with Max, and programmed within Pico, and the physical part came together.

**Notes:** 

251201

* Created a generative system on Max for one motor and two magnets.

Explanation of the patch:

Motor:

The five notes, duration, note length and pause are randomised.

Magnet:

Used 'coll' object to store seven types of instructions of magnet type  and duration. 'urn' randomly chooses the number without repeating, and once urn finishes it sends a bang to clear, and bang urn again to restart. The system loops indefinitely, so there is a gate to stop the operation.

251202

* I don't need the magnet box, I just need strings to attach it.

251203

* Started to work in the quiet room, and finally I started to have some progress.
* The physical components took shape.

251204

The goal of my final project is not make a polished work, but understanding the mechanisms and code that I can develop further in different directions for performance and installations.

```
//pico as a mini sequencer

#define STEP_PIN 6 //each HIGH-LOW is one microstep
#define DIR_PIN 7 //forward or reverse
#define EN_PIN 21  //enable or disable driver (low = on, high = off)

int motorspeed;

// ARRAY storing the notes. higher number is faster, 0 is off, negative - reverse direction
int motor1_notes[] = {
  0,0,0,0,-127,-127,-127,-127,
  0,0,127,127,0,127,0,127,
  -127,-127,-127,-127,0,0,0,0,
  0,0,0,0,-127,-127,-127,-127,
  0,0,127,127,0,127,0,127,
  -127,-127,-127,-127,0,0,0,0,
  0,0,0,0,0,0,0,0, // 1 magnet 1 solo
  0,0,0,0,0,0,0,0, // 2 magnet 1 solo
  60,60,0,0,0,0,0, // 3 motor solo - tiny spin & stop
  60,60,60,60,0,0,0,0, // 4 motor solo - spin & stop
  90,90,90,90,90,90,90,90, // 5 motor spin 
  67,64,62,60,67,64,62,60, // 6 motor spin 
  90,90,90,90,90,90,90,90, // 7 motor spin - magnet 2 chimes in
  0,0,0,0,-80,-60,-62,-60,-57, // 8 motor stop & backward 
  -76,-76,-76,-76,-72,-72,-72,-76, // 9 motor backward
  0,0,0,0,0,0,0,0, // 10 silent
  0,0,0,0,0,0,0,0, // 11 all magent same action
  0,0,0,0,0,0,0,0, // 12 magnets solo
  70,70,70,80,60,90,90,90, // 13 ALL
  70,70,70,80,60,90,90,90, // 14 ALL
  80,70,127,127,80,80,127,127 // 15 FINALLE
  };

int magnet1_notes[] = {
  1,0,0,0,0,0,1,0,
  0,0,1,0,1,0,1,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  1,0,1,0,1,0,0,1,
  1,1,0,0,1,0,1,1,
  1,1,0,0,1,0,1,1,
  1,0,1,0,1,0,0,1,
  1,1,0,0,1,0,1,1,
  1,0,1,0,0,1,0,1
  };

int magnet2_notes[] = {
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,1,0,0,1,0,1,1, 
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  1,0,1,0,1,0,0,1,
  1,1,0,0,1,0,1,1,
  1,0,1,0,1,0,0,1,
  1,1,0,0,1,0,1,1,
  1,0,1,0,0,0,0,1
  };

int magnet3_notes[] = {
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,1,0,1,0,1,0,1,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  0,0,0,0,0,0,0,0,
  1,0,1,0,1,0,0,1,
  1,0,1,0,1,0,1,0,
  0,1,0,1,0,1,0,1,
  1,1,1,0,1,0,1,0,
  1,0,1,0,1,0,1,0
  };

int counter = 0; // VARIABLE - a counter steps through the sequence - where we are in the sequence?
int stepTime = 500; //duration of each step / bpm / tempo 500ms = 120bpm. every stepTime milliseconds move counter by 1 

//when should i step to the new step - millis
//one variable called currentTime
long currentTime = 0; //when has miliseconds pasted - start a stop watch, start tracking - lap.
long sampledTime = 0;

void setup() {

  //step pin
  pinMode(6, OUTPUT);
  //direction pin
  pinMode(7, OUTPUT);
  //enable pin
  pinMode(21, OUTPUT);

  //start with motors disabled. High=1. Low=0.
  digitalWrite(EN_PIN, HIGH);

  //magnet 1
  pinMode(18, OUTPUT);
  //magnet 2
  pinMode(19, OUTPUT);
  //magnet 3
  pinMode(20, OUTPUT); 

  digitalWrite(19, LOW);
  digitalWrite(18, LOW);

  // initialize serial communication:
  Serial.begin(9600);
}

void loop() {
  currentTime = millis();
  if(currentTime - sampledTime > stepTime){
    counter++;
    if(counter > 120){ //resets the timer
      counter = 0; 
    } 
    sampledTime = currentTime;
  }
  //has stepTime passed? if yes, advance to next note.

  int note = motor1_notes[counter]; // motor1_notes[] prints 0,1,2 motor1_notes[counter] prints the notes 127,60,84

  if (note == 0){ 
    digitalWrite(EN_PIN, HIGH); //motor OFF
  }else{
    digitalWrite(EN_PIN, LOW); //motor ON
  }

  Serial.println(note);

  if (note < 0){
  digitalWrite(DIR_PIN, HIGH);
  }else if (note > 0){
  digitalWrite(DIR_PIN, LOW);
  }
  
  //note = 127. motor speed = 0. note = 0 motorspeed = 45000. map flips the direction 
    motorspeed = map(abs(note), 127, 0,0, 45000);
  //motorspeed = map(abs(note), 127, 0,1000, 45000/16);

  //having a gate value - 0 or 1. only do the onestep function when the gate is on.
  //sends a single microstep
  oneStep();

  //magnet 1
  if (magnet1_notes[counter] == 0){
    digitalWrite(18, HIGH);
  }else{
    digitalWrite(18, LOW);
  }
  //magnet 2
    if (magnet2_notes[counter] == 0){
    digitalWrite(19, HIGH);
  }else{
    digitalWrite(19, LOW);
  }
  //magnet 3
    if (magnet3_notes[counter] == 0){
    digitalWrite(20, HIGH);
  }else{
    digitalWrite(20, LOW);
  }
}

void oneStep() {
  digitalWrite(STEP_PIN, HIGH);
  //delayMicroseconds(1);
  delayMicroseconds(1);
  digitalWrite(STEP_PIN, LOW);
  delayMicroseconds(1);
  delayMicroseconds(motorspeed);
}

void handleNoteOn(byte channel, byte pitch, byte velocity) {

  if (channel == 1) {
    digitalWrite(EN_PIN, LOW);  //low means 0 means ON
    motorspeed = map(pitch, 127, 0, 0, 45000);
    Serial.println(motorspeed);
  }

  if (channel == 2) {
    if (pitch == 60) digitalWrite(19, HIGH);
    if (pitch == 61) digitalWrite(18, HIGH);
    if (pitch == 62) digitalWrite(20, HIGH);
  }

  // Log when a note is pressed.
  Serial.print("Note on: channel = ");
  Serial.println(channel);

  Serial.print(" pitch = ");
  Serial.println(pitch);

  Serial.print(" velocity = ");
  Serial.println(velocity);
}

void handleNoteOff(byte channel, byte pitch, byte velocity) {
  if (channel == 1) {
    digitalWrite(EN_PIN, HIGH);
  }
  if (channel == 2) {
    if (pitch == 60) digitalWrite(19, LOW);
    if (pitch == 61) digitalWrite(18, LOW);
    if (pitch == 62) digitalWrite(20, LOW);
  }
}
```

Using pico as a sequencer

* stepTime is the tempo
* A counter that steps through the sequence
* Arrays store motor/magnet notes
* Every X milliseconds, the counter moves to the next step in the array
* What does millis do in this case?

Reorganising and simplifying the cables

* for the three magnets I was getting power directly from the power source. I changed it to one goes into the breadboard, and the connecting to the magnets.
* be careful that 5v shouldn't be connected to 3.3v
* ground needs to be all connected

The happy accident:

0 0 0 0 127 127 127 127

→ motor failing to do its job sounds so good.

To make everything more quiet:\
Why does micro stepping make everything smoother and quieter?

251213

* I changed to silent driver independently!! Previously, I had help from Oskars while connecting to the motor and motor driver. This time I drew a diagram to understand each connection, ground, power, output and input, and measured the resistance with the multi-meter.
* The silent driver micro stepping stopped working at some point. Then, I figured out the resistance was too high that the motor couldn't spin.
* Silent driver - with the non-micro stepping code, it creates loud sounds. Then it's silent when I use the micro stepping code.

 \
**Project Description**

For the final project, I created a preliminary prototype of a sounding object to be placed within a bookshelf at a library. The project explores the ideas of micro sounds and sound installation without speakers, focusing on the sonic materiality of book-related objects, such as paper clips and cardboard. 

The current prototype consists of one stepper motor and three electromagnets that produce sounds from magnet tapping and cardboard friction. 

Rather than aiming for polished work, I focused on building two working systems for installation (Arduino) and performance (Max), which the composition can be further developed after the course.

**What did I learn from this course?**

The first lesson about electricity (voltage, current, resistance) has cleared many confusion I previously had. Understanding these fundamentals has helped and will help me to work more independently - researching online, reading diagrams of the component, reading data sheets, understanding each output and input, calculating resistance, schematic diagram, power supply voltage, etc. 

**Dilemmas between a fixed idea and unpolished work**

I didn’t have a clear goal before I began, soon after I realised it was too broad so I narrowed it down to Morse Code and paper pins. The system was working, but I realised if I am not using Morse Code in the end, then I shouldn’t spend most of the time on composition, so I threw the idea away. Even though I was frustrated about having unpolished work in the last two weeks, I begin to think this is only the beginning, and not the end. 

**Personal Reflections**

The main problem I had was exhaustion from the amount of work and deadlines I had outside of this class, that I no longer had the capacity to think creatively. Also, I get too distracted in open work space, because I was curious about everyone's project and get caught up in conversations. I need to lock myself inside a quiet room to think and work, and finally start making progress…

**What did I discover?**

One of the main reasons that has stopped me from creating has been feeling guilty to use materials, such as wood, metal, etc. Creating a prototype with found objects that I can dissemble the elements and the borrowed materials can return to their place of origin. No materials are being wasted was a relieve. I also like to think about the afterlife of materials and question what happens to the materials after the installation.

I realised I like to use bandage in my work, because reality hurts. 

**Next steps**

→ Understanding millis with two motors

→ Working more independently by myself, I could solve simple problems by myself with lesser time limit and stress. 

→ Further research about library catalogues, functionality of bookshelf, what is my language.

→ Help Matti to organise the library catalogues → perhaps it will spark some ideas
