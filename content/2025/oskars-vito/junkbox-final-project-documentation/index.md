---
title: Junkbox - final project documentation
date: 2025-11-25T17:46:00.000+02:00
authors:
  - Oskars Vito
image: jukebox.jpg
bgimage: jukebox.jpg
showBgImage: true
---
# Junkbox

The junkbox is a 4x stepper motor MIDI to analog vibration box inspired by Koka Nikoladze.

As a bonus thing to play with, for the Computational Art & Design part, I created a P5js sketch that is inspired by the Omnichord instrument.



### Links to the project deliverables:

Computational Art and Design:

https://www.dropbox.com/scl/fo/xqhn2izlfye7ei80j64xj/AHm7kx0BKKnESYaUEB7pV00?rlkey=dhxt2x76qdi7vtee0xkzqh8pk&st=zyhnfjod&dl=0

Physical Computing:

https://www.dropbox.com/scl/fo/bd007ub9x9nxbs58ckr7n/AFvD2hyr7uq1clMtTXHshm4?rlkey=k95fyvhv91okjrcd43md3sg42&st=5dmhspp2&dl=0



![](physical-computing-_-computational-art-final-project.jpg)

{{<youtube PXiHB2uNEgs>}}

## Motor tuning code

```
//including the MIDI libraries
#include <Arduino.h>
#include <Adafruit_TinyUSB.h>
#include <MIDI.h>

// USB MIDI object - library accessing
Adafruit_USBD_MIDI usb_midi;

// Create a new instance of the Arduino MIDI Library,
// and attach usb_midi as the transport.
MIDI_CREATE_INSTANCE(Adafruit_USBD_MIDI, usb_midi, MIDI);

//          SETTING UP THE MOTORS
int potentiometer;
int speed;

//  MOTOR 1
#define EN_PIN_1 16  // UPDATED from 5

#define STEP_PIN_1 21
#define DIR_PIN_1 20

//  MOTOR 2
#define EN_PIN_2 22  // UPDATED from 4

#define STEP_PIN_2 19
#define DIR_PIN_2 18

//  MOTOR 3
#define EN_PIN_3 15  // UPDATED from 1

#define STEP_PIN_3 12
#define DIR_PIN_3 13

//  MOTOR 4 (silent driver)
#define EN_PIN_4 0

#define STEP_PIN_4 3
#define DIR_PIN_4 2

const bool motorDirection = LOW; //you can use this to change the motor direction, comment out if you aren't using it.

// THE MILLIS STUFF
//Index 0 is not used.
unsigned long motorSpeeds[] = {0, 0, 0, 0, 0}; //holds the speed value of the motors. 
unsigned long prevStepMicros[] = {0, 0, 0, 0, 0}; //last time
unsigned long startMicros;

unsigned long lastTime; //Will store the time that the last event occured.
int TIME = 1000; // Number of milliseconds for millis()

//SETUP
void setup() {
  analogReadResolution(12);
  
  // Initialize enable pins UPDATED
  pinMode(16, OUTPUT);  // EN_PIN_1
  pinMode(22, OUTPUT);  // EN_PIN_2
  pinMode(15, OUTPUT);  // EN_PIN_3
  pinMode(0, OUTPUT);   // EN_PIN_4
  
  lastTime = millis(); //variable that holds the start time
  
  Serial.begin(9600);
  // Manual begin() is required on core without built-in support e.g. mbed rp2040
  if (!TinyUSBDevice.isInitialized()) {
    TinyUSBDevice.begin(0);
  }
  usb_midi.setStringDescriptor("TinyUSB MIDI");

  // Initialize MIDI, and listen to all MIDI channels
  // This will also call usb_midi's begin()
  MIDI.begin(MIDI_CHANNEL_OMNI);
  MIDI.turnThruOff(); //turning off the sending of the recieved MIDI info

  // If already enumerated, additional class driverr begin() e.g msc, hid, midi won't take effect until re-enumeration
  if (TinyUSBDevice.mounted()) {
    TinyUSBDevice.detach();
    delay(10);
    TinyUSBDevice.attach();
  }
  digitalWrite(EN_PIN_1, HIGH);  // Start with motors disabled
  digitalWrite(EN_PIN_2, HIGH);
  digitalWrite(EN_PIN_3, HIGH);
  digitalWrite(EN_PIN_4, HIGH);

  // Attach the handleNoteOn function to the MIDI Library. It will
  // be called whenever the Bluefruit receives MIDI Note On messages.
  MIDI.setHandleNoteOn(handleNoteOn);

  // Do the same for MIDI Note Off messages.
  MIDI.setHandleNoteOff(handleNoteOff);

  //MOTOR NR 1.
  //enable pin UPDATED
  pinMode(16, OUTPUT);
  //step pin
  pinMode(21, OUTPUT);
  //direction pin
  pinMode(20, OUTPUT);

  //MOTOR NR 2.
  //enable pin UPDATED
  pinMode(22, OUTPUT);
  //step pin
  pinMode(19, OUTPUT);
  //direction pin
  pinMode(18, OUTPUT);

  //MOTOR NR 3.
  //enable pin UPDATED
  pinMode(15, OUTPUT);
  //step pin
  pinMode(12, OUTPUT);
  //direction pin
  pinMode(13, OUTPUT);

  //MOTOR NR 4.
  //enable pin
  pinMode(0, OUTPUT);
  //step pin
  pinMode(3, OUTPUT);
  //direction pin UPDATED
  pinMode(2, OUTPUT);  // Changed from 4 to 2 to match DIR_PIN_4

  digitalWrite(DIR_PIN_1, motorDirection);
  digitalWrite(DIR_PIN_2, motorDirection);
  digitalWrite(DIR_PIN_3, motorDirection);
  digitalWrite(DIR_PIN_4, motorDirection);

  //reading from the potentiometer
  pinMode(26, INPUT);

}



void loop() {
  
  
  // read any new MIDI messages
  MIDI.read();

  //updating the MIDI
  #ifdef TINYUSB_NEED_POLLING_TASK
  // Manual call tud_task since it isn't called by Core's background
  TinyUSBDevice.task();
  #endif

  
  // calling the one step function for each stepper motor
  oneStep(1, STEP_PIN_1);
  oneStep(2, STEP_PIN_2);
  oneStep(3, STEP_PIN_3);
  oneStep(4, STEP_PIN_4);


  potentiometer = analogRead(26);
  speed = map(potentiometer, 0, 4095, 16, 500);
  
  
}

//MOTOR STEP FUNCTION

void oneStep(byte motorNumber, byte stepPin) {
  if (motorSpeeds[motorNumber] == 0) return; // Exit early if motor should be stopped
  
  unsigned long currentMicros = micros();
  
  // Check if enough time has elapsed (handles overflow correctly)
  if (currentMicros - prevStepMicros[motorNumber] >= motorSpeeds[motorNumber])
  {
    prevStepMicros[motorNumber] = currentMicros; // Use current time instead of adding
    digitalWrite(stepPin, HIGH);
    delayMicroseconds(2); // Small delay to ensure the driver registers the pulse
    digitalWrite(stepPin, LOW);
    
  }
}



void handleNoteOn(byte channel, byte pitch, byte velocity) {
  if(channel == 1){
    digitalWrite(EN_PIN_1, 0);
    //gate_1 = true; //we have signal
    //update motor speed
    motorSpeeds[1] = speed;
    Serial.println(speed);
  }
  if(channel == 2){
    digitalWrite(EN_PIN_2, 0);
    //gate_2 = true; //we have signal
    //update motor speed
    motorSpeeds[2] = speed;
    Serial.println(speed);
  }
  if(channel == 3){
    digitalWrite(EN_PIN_3, 0);
    //gate_3 = true; //we have signal
    //update motor speed
    motorSpeeds[3] = speed;
    Serial.println(speed);
  }
  if(channel == 4){
    digitalWrite(EN_PIN_4, 0);
    //gate_4 = true; //we have signal
    //update motor speed
    motorSpeeds[4] = speed;
    Serial.println(speed);
  }
}

void handleNoteOff(byte channel, byte pitch, byte velocity) {
 
  if(channel == 1){
    //gate_1  = false;
    //motorSpeeds[1] = motorSpeedPitch_1[0];
    digitalWrite(EN_PIN_1, 1);
  }
  if(channel == 2){
    digitalWrite(EN_PIN_2, 1);
    //gate_2  = false;
    //motorSpeeds[2] = motorSpeedPitch_2[0];
  }
  if(channel == 3){
    digitalWrite(EN_PIN_3, 1);
    //gate_3  = false;
    //motorSpeeds[3] = motorSpeedPitch_3[0];
  }
  if(channel == 4){
    digitalWrite(EN_PIN_4, 1);
    //gate_4  = false;
    //motorSpeeds[4] = motorSpeedPitch_4[0];
  }

}
```

## This is the presentation version of the code. (V15)

```

//including the MIDI libraries
#include <Arduino.h>
#include <Adafruit_TinyUSB.h>
#include <MIDI.h>

// USB MIDI object - library accessing
Adafruit_USBD_MIDI usb_midi;

// Create a new instance of the Arduino MIDI Library,
// and attach usb_midi as the transport.
MIDI_CREATE_INSTANCE(Adafruit_USBD_MIDI, usb_midi, MIDI);

//          SETTING UP THE MOTORS




//  MOTOR 1
#define EN_PIN_1 16 //was gp 5

#define STEP_PIN_1 21
#define DIR_PIN_1 20

//  MOTOR 2
#define EN_PIN_2 22 //was gp 4

#define STEP_PIN_2 19
#define DIR_PIN_2 18

//  MOTOR 3
#define EN_PIN_3 15 //was gp 1

#define STEP_PIN_3 12
#define DIR_PIN_3 13

//  MOTOR 4 (silent driver)
#define EN_PIN_4 0

#define STEP_PIN_4 3
#define DIR_PIN_4 2

// uint32_t motorSpeed_1; //motor one speed
// uint32_t motorSpeed_2; // motor two speed
// bool gate_1; //making the motor spin and stop TRUE = NOTE IS ON
// bool gate_2; //making the motor spin and stop TRUE = NOTE IS ON
// bool gate_3; //making the motor spin and stop TRUE = NOTE IS ON
// bool gate_4; //making the motor spin and stop TRUE = NOTE IS ON
const bool motorDirection = LOW; //you can use this to change the motor direction, comment out if you aren't using it.

// THE MILLIS STUFF
//Index 0 is not used.
unsigned long motorSpeeds[] = {0, 0, 0, 0, 0}; //holds the speed value of the motors. 
unsigned long prevStepMicros[] = {0, 0, 0, 0, 0}; //last time
unsigned long startMicros;

unsigned long lastEvent; //Will store the time that the last event occured.
int notMoving = 0;

// Motor 1 tuning array - CORRECTED based on calibration sheets
const uint32_t motorSpeedPitch_1[128] = {
  // Octave -1 (MIDI 0-11) - extrapolated backwards from A1=36316
  290528, 274176, 258688, 244096, 230336, 217472, 205376, 193984, 183296, 173056, 163392, 154176,
  
  // Octave 0 (MIDI 12-23) - extrapolated backwards from A1=36316
  145408, 137088, 129344, 122048, 115168, 108736, 102688, 96992, 91648, 86528, 81696, 77088,
  
  // Octave 1 (MIDI 24-35): C1-B1
  72704, 68544, 64672, 61024, 57584, 54368, 51344, 48496, 45792, 36316, 34296, 32314,
  
  // Octave 2 (MIDI 36-47): C2-B2
  30527, 28871, 27098, 25739, 24246, 22825, 21653, 20302, 19516, 18432, 17242, 16219,
  
  // Octave 3 (MIDI 48-59): C3-B3
  15416, 14500, 13750, 12971, 12166, 11528, 10829, 10293, 9651, 9071, 8599, 8117,
  
  // Octave 4 (MIDI 60-71): C4-B4
  7619, 7205, 6804, 6384, 6074, 5721, 5423, 5103, 4803, 4536, 4314, 4026,
  
  // Octave 5 (MIDI 72-83): C5-B5
  3801, 3600, 3407, 3209, 3037, 2865, 2706, 2507, 2405, 2269, 2141, 2000,
  
  // Octave 6 (MIDI 84-95): C6-B6
  1907, 1803, 1703, 1607, 1519, 1430, 1353, 1274, 1205, 1136, 1070, 1010,
  
  // Octave 7 (MIDI 96-107): C7-B7 - extrapolated forward
  954, 901, 851, 803, 758, 716, 676, 638, 603, 569, 537, 507,
  
  // Octave 8 (MIDI 108-119): C8-B8 - extrapolated forward
  479, 452, 427, 403, 381, 359, 339, 320, 302, 285, 269, 254,
  
  // Octave 9-10 (MIDI 120-127): C9-G10 - extrapolated forward
  240, 226, 214, 202, 191, 180, 170, 161
};


// Motor 2 tuning array - CORRECTED based on calibration sheets
const uint32_t motorSpeedPitch_2[128] = {
  // Octave -1 (MIDI 0-11) - extrapolated backwards from A1=36316
  290528, 274176, 258688, 244096, 230336, 217472, 205376, 193984, 183296, 173056, 163392, 154176,
  
  // Octave 0 (MIDI 12-23) - extrapolated backwards from A1=36316
  145408, 137088, 129344, 122048, 115168, 108736, 102688, 96992, 91648, 86528, 81696, 77088,
  
  // Octave 1 (MIDI 24-35): C1-B1
  72704, 68544, 64672, 61024, 57584, 54368, 51344, 48496, 45792, 36316, 34296, 32314,
  
  // Octave 2 (MIDI 36-47): C2-B2
  30527, 28871, 27098, 25739, 24246, 22825, 21653, 20302, 19516, 18432, 17242, 16219,
  
  // Octave 3 (MIDI 48-59): C3-B3
  15416, 14500, 13750, 12971, 12166, 11528, 10829, 10293, 9651, 9071, 8599, 8117,
  
  // Octave 4 (MIDI 60-71): C4-B4
  7619, 7205, 6804, 6384, 6074, 5721, 5423, 5103, 4803, 4536, 4314, 4026,
  
  // Octave 5 (MIDI 72-83): C5-B5
  3801, 3600, 3407, 3209, 3037, 2865, 2706, 2547, 2405, 2269, 2141, 2000,
  
  // Octave 6 (MIDI 84-95): C6-B6
  1907, 1803, 1703, 1607, 1519, 1430, 1353, 1274, 1205, 1136, 1070, 1010,
  
  // Octave 7 (MIDI 96-107): C7-B7 - extrapolated forward
  954, 901, 851, 803, 758, 716, 676, 638, 603, 569, 537, 507,
  
  // Octave 8 (MIDI 108-119): C8-B8 - extrapolated forward
  479, 452, 427, 403, 381, 359, 339, 320, 302, 285, 269, 254,
  
  // Octave 9-10 (MIDI 120-127): C9-G10 - extrapolated forward
  240, 226, 214, 202, 191, 180, 170, 161
};

// Motor 3 tuning array - CORRECTED based on calibration sheets
// Motor 3 tuning array - calibrated from measurements
const uint32_t motorSpeedPitch_3[128] = {
  // Octave -1 (MIDI 0-11) - extrapolated
  77048, 72696, 68544, 64656, 61008, 57552, 54312, 51264, 48384, 45672, 43104, 40680,
  
  // Octave 0 (MIDI 12-23): C0-B0
  // Extrapolating up to G#0=9631 (MIDI 20)
  38524, 36348, 34272, 32328, 30504, 28776, 27156, 25632, 9631, 9087, 8592, 8122,
  
  // Octave 1 (MIDI 24-35): C1-B1
  // EXACT from image: C1=7653, C#1=7227, D1=6816, D#1=6429, E1=6066, F1=5726, F#1=5403, G1=5103, G#1=4816, A1=4544, A#1=4296, B1=4061
  7653, 7227, 6816, 6429, 6066, 5726, 5403, 5103, 4816, 4544, 4296, 4061,
  
  // Octave 2 (MIDI 36-47): C2-B2
  3827, 3614, 3408, 3215, 3033, 2863, 2702, 2552, 2408, 2272, 2148, 2031,
  
  // Octave 3 (MIDI 48-59): C3-B3
  1914, 1807, 1704, 1608, 1517, 1432, 1351, 1276, 1204, 1136, 1074, 1016,
  
  // Octave 4 (MIDI 60-71): C4-B4
  957, 904, 852, 804, 759, 716, 676, 638, 602, 568, 537, 508,
  
  // Octave 5 (MIDI 72-83): C5-B5
  479, 452, 426, 402, 380, 358, 338, 319, 301, 284, 269, 254,
  
  // Octave 6 (MIDI 84-95): C6-B6
  240, 226, 213, 201, 190, 179, 169, 160, 151, 142, 135, 127,
  
  // Octave 7 (MIDI 96-107): C7-B7
  120, 113, 107, 101, 95, 90, 85, 80, 76, 71, 68, 64,
  
  // Octave 8 (MIDI 108-119): C8-B8
  60, 57, 54, 51, 48, 45, 43, 40, 38, 36, 34, 32,
  
  // Octave 9-10 (MIDI 120-127): C9-G10
  30, 29, 27, 26, 24, 23, 22, 20
};

const uint32_t motorSpeedPitch_4[128] = {
  3591, 3400, 3218, 3047, 2884, 2730, 2585, 2447, 2316, 2193, 2076, 1965,
  1860, 1761, 1667, 1578, 1494, 1414, 1339, 1268, 1200, 1136, 1070, 1010,
  955, 897, 847, 801, 754, 714, 675, 634, 599, 565, 533, 505,
  475, 446, 423, 399, 376, 353, 334, 317, 299, 280, 264, 250,
  235, 220, 209, 197, 185, 174, 165, 156, 146, 138, 130, 122,
  115, 108, 102, 96, 91, 84, 77, 74, 68, 63, 60, 57,
  54, 51, 46, 44, 38, 36, 35, 33, 32, 30, 26, 23,
  20, 20, 19, 19, 19, 18, 17, 17, 16, 15, 15, 14,
  13, 13, 12, 12, 11, 11, 10, 10, 10, 9, 9, 8,
  8, 8, 7, 7, 7, 6, 6, 6, 6, 5, 5, 5,
  5, 5, 4, 4, 4, 4, 4, 4
};

//SETUP
// put your setup code here, to run once:
void setup() {
  pinMode(16, OUTPUT);
  pinMode(22, OUTPUT);
  pinMode(15, OUTPUT);
  pinMode(0, OUTPUT);
  lastEvent = millis(); //variable that holds the start time
 
  Serial.begin(9600);
  // Manual begin() is required on core without built-in support e.g. mbed rp2040
  if (!TinyUSBDevice.isInitialized()) {
    TinyUSBDevice.begin(0);
  }
  usb_midi.setStringDescriptor("TinyUSB MIDI");

  // Initialize MIDI, and listen to all MIDI channels
  // This will also call usb_midi's begin()
  MIDI.begin(MIDI_CHANNEL_OMNI);
  MIDI.turnThruOff(); //turning off the sending of the recieved MIDI info

  // If already enumerated, additional class driverr begin() e.g msc, hid, midi won't take effect until re-enumeration
  if (TinyUSBDevice.mounted()) {
    TinyUSBDevice.detach();
    delay(10);
    TinyUSBDevice.attach();
  }
  digitalWrite(EN_PIN_1, HIGH);  // Start with motors disabled
  digitalWrite(EN_PIN_2, HIGH);
  digitalWrite(EN_PIN_3, HIGH);
  digitalWrite(EN_PIN_4, HIGH);

  // Attach the handleNoteOn function to the MIDI Library. It will
  // be called whenever the Bluefruit receives MIDI Note On messages.
  MIDI.setHandleNoteOn(handleNoteOn);

  // Do the same for MIDI Note Off messages.
  MIDI.setHandleNoteOff(handleNoteOff);

      //MOTOR NR 1.
  //enable pin
  pinMode(16, OUTPUT);
  //step pin
  pinMode(21, OUTPUT);
  //direction pin
  pinMode(20, OUTPUT);

      //MOTOR NR 2.
  //enable pin
  pinMode(22, OUTPUT);
  //step pin
  pinMode(19, OUTPUT);
  //direction pin
  pinMode(18, OUTPUT);

      //MOTOR NR 3.
  //enable pin
  pinMode(15, OUTPUT);
  //step pin
  pinMode(12, OUTPUT);
  //direction pin
  pinMode(13, OUTPUT);

      //MOTOR NR 4.
  //enable pin
  pinMode(0, OUTPUT);
  //step pin
  pinMode(3, OUTPUT);
  //direction pin
  pinMode(4, OUTPUT);

  digitalWrite(DIR_PIN_1, motorDirection);
  digitalWrite(DIR_PIN_2, motorDirection);
  digitalWrite(DIR_PIN_3, motorDirection);
  digitalWrite(DIR_PIN_4, motorDirection); //and this one too. */


}



void loop() {
  
  
  // read any new MIDI messages
  MIDI.read();

    //updating the MIDI
      #ifdef TINYUSB_NEED_POLLING_TASK
      // Manual call tud_task since it isn't called by Core's background
      TinyUSBDevice.task();
      #endif

  
  // calling the one step function for each stepper motor
  oneStep(1, STEP_PIN_1);
  oneStep(2, STEP_PIN_2);
  oneStep(3, STEP_PIN_3);
  oneStep(4, STEP_PIN_4);





}
//MOTOR STEP FUNCTION
void oneStep(byte motorNumber, byte stepPin) {
  if ((micros() - prevStepMicros[motorNumber] >= motorSpeeds[motorNumber]) && (motorSpeeds[motorNumber] != 0))  //test whether the period has elapsed and don't use index 0
  {
    prevStepMicros[motorNumber] += motorSpeeds[motorNumber];
    digitalWrite(stepPin, HIGH);
    digitalWrite(stepPin, LOW);
  }
}




void handleNoteOn(byte channel, byte pitch, byte velocity) {
  if(channel == 1){
    digitalWrite(EN_PIN_1, 0);
    //gate_1 = true; //we have signal
    //update motor speed
    motorSpeeds[1] = motorSpeedPitch_1[pitch];
  }
    if(channel == 2){
    digitalWrite(EN_PIN_2, 0);
    //gate_2 = true; //we have signal
    //update motor speed
    motorSpeeds[2] = motorSpeedPitch_2[pitch];
  }
   if(channel == 3){
    digitalWrite(EN_PIN_3, 0);
    //gate_3 = true; //we have signal
    //update motor speed
    motorSpeeds[3] = motorSpeedPitch_3[pitch];
  }
   if(channel == 4){
    digitalWrite(EN_PIN_4, 0);
    //gate_4 = true; //we have signal
    //update motor speed
    motorSpeeds[4] = motorSpeedPitch_4[pitch];
  }

}

void handleNoteOff(byte channel, byte pitch, byte velocity) {
 
  if(channel == 1 ){
    //gate_1  = false;
    //motorSpeeds[1] = motorSpeedPitch_1[0];
    digitalWrite(EN_PIN_1, 1);
  }
  if(channel == 2 ){
    digitalWrite(EN_PIN_2, 1);
    //gate_2  = false;
    //motorSpeeds[2] = motorSpeedPitch_2[0];
  }
  if(channel == 3 ){
    digitalWrite(EN_PIN_3, 1);
    //gate_3  = false;
    //motorSpeeds[3] = motorSpeedPitch_3[0];
  }
  if(channel == 4 ){
    digitalWrite(EN_PIN_4, 1);
    //gate_4  = false;
    //motorSpeeds[4] = motorSpeedPitch_4[0];
  }

}

```
