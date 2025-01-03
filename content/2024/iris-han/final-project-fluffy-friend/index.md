---
title: "Final Project: Fluffy Friend"
date: 2024-12-09T14:24:00.000Z
authors:
  - Iris Han
image: featured.jpg
bgimage: background.jpg
showBgImage: true
---
> The inspiration comes from "socializing" which we do every day. After socializing with people for so many years, I suddenly feel that socializing with a fluffy unknown creature might be also a good choice. 
>
> <https://miro.com/welcomeonboard/bTZTRlBURkRvTHRGLzFsa3NvaFU1ZmIwMDZ6MGF0KzV3WWxoWk1LdVg1WWhEWDFHUTNCVXIycmttQTdCZzNDNi9oVDVFQzcvNnlFU3Zwd3BaL1phTGtIaWRuYU5xcG5rZUJIM25iQSs1QXY3TXJiRC94Qy9hLzUwR0gwMlRPS0chZQ==?share_link_id=534445760282>

I used Arduino to create some work in the past time, but only in some simple ways. I'm so happy to learn physical computing because this is an opportunity to learn this skill formally and systematically. At the beginning of this project, I want to try more Arduino components and use the wonderful resources from this course. Additionally, I am so addicted to plush toys and fluffy stuff. So the concept of this project quickly came up in my head. Let me make some soft fluffy robot! 

![Here is the sketch](sketch2.jpg "Draft")

In the process of designing this robot, I referred to some simple robot toys.

![](image.png "https://miro.com/app/board/uXjVLI1K7Ds=/?moveToWidget=3458764606176414540&cot=14")

![](image-1-.png "https://miro.com/app/board/uXjVLI1K7Ds=/?moveToWidget=3458764606177258504&cot=14")

Thinking about the structure and materials was a big challenge in the first stage. I realized I didn't know how and where I could buy all the materials. I had just arrived in Finland and was completely clueless about how to source materials. My previous experience was not applicable here. So I felt a little desperate at first because I didn’t even know where to buy cotton for filling. However, I finally solved this problem! I got so much help from my classmates and Matti! I finally got nice fabric, metal mesh, needle!

![](屏幕截图-2024-12-09-213204.png)

In the early stage of the design, I tried my best to make the robot more humane and act like a pet or a companion, so the voice, body language, movements, and expressions were all factors I needed to consider.

![](屏幕截图-2024-12-11-234527.png)

Firstly, I tried to write very simple code to prototype the functions of a robot. 

![](屏幕截图-2024-11-29-101302.png)

Then starting to build it on an Arduino board.

![](微信图片_20241209214946.jpg)

Because I want to make a very soft robot, the sensors shouldn't be too hard. I made this soft pressure sensor by using the conductive fabric.

![](微信图片_20241209214958.jpg)

In the beginning, I planned to use an LED display to show the robot's face and express different emotions. Due to the material and budget, I finally chose this Neo pixel ring as the blush on the cheek. 

When people touch or squeeze the arms(pressure sensors), neo-pixel rings will turn on.

![](微信图片_20241209223005.jpg)

![](微信图片_20241209223048.jpg "This is the first attempt at combining the pressure sensor and the robot's arm. ")

After testing all parts of the robot, I realized that I used so many components. There are 4 servo motors, 2 neo pixels rings, 3 pressure sensors, 2 distance sensors and 1 speaker, which were very challenging and unfamiliar for me. Also, I was not very clear with all the names in English. Everything seemed difficult for me. 

However, after a week of exploration and learning, I gradually understood the connection and structure between my work and Arduino. Then everything became clearer, even the progress was quite tortuous. 

Unfortunately, neo pixels' and servo motors' libraries are conflicted with each other. I decided to separate functions and components into two Arduino boards(Arduino Uno Wifi 4 and Arduino Uno). 

![](微信图片_20241209223014.jpg)







![](微信图片_20241209215009.jpg)



Physical building stage:

I used laser cut to make the bottom base and head base. 

Using metal mesh and iron wire to make the structure(skeleton). 

I also encountered some difficulties when assembling all the parts, such as it was difficult to fix the servo firmly to the frame, or the soft pressure sensor's value became unstable due to the cotton filling in the arm. It caused neo-pixel rings to blink. To solve the problem of flickering neon lights, Matti suggested changing the fixed value to the readable value. 



```c
int sensor1Min = 0;
int sensor1Max = 0;
int sensor2Min = 0;
int sensor2Max = 0;
int sensor3Min = 0;
int sensor3Max = 0;

 if (fsrValue1 > fsrValue2 && fsrValue1 > fsrValue3){
    targetBrightness = map(fsrValue1, sensor1Min+20, sensor1Max, 0, 255);
  }
  if (fsrValue2 > fsrValue1 && fsrValue2 > fsrValue3){
    targetBrightness = map(fsrValue2,sensor2Min+7, sensor2Max, 0, 255);
  }
  if (fsrValue3 > fsrValue1 && fsrValue3 > fsrValue2){
    targetBrightness = map(fsrValue3, sensor3Min+20, sensor3Max, 0, 255);
  }
```

![](微信图片_20241209223040.jpg)

![](微信图片_20241209223111.jpg)

Soldering stage!

I like soldering because it saved me from my messy wires and breadboard. Everything became so clear!!!

![](微信图片_20241209223130.jpg)



![](微信图片_20241209223147.jpg)

![](微信图片_20241209223104.jpg "Process")

![](微信图片_20241209223155.jpg "Head~")

The last part I completed was the speaker. I wanted the robot to make different sounds based on the interaction between people and robots. However the trigger used by the speaker always had various problems.

Doing this also made me realize that it is important to explore different libraries and hardware, and search for open source and helpful codes on the website for in-depth learning. I think the main reason why I felt that doing this project was difficult in the early stage was that I was not flexible enough in using Arduino. After a week of doing it, I felt that I could understand both the circuit and the code more easily.

This class helped me a lot. I know that this course is relatively hardcore, but what I learned from it cannot be compared with the pressure in the process.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Finally, this robot got a fancy name----Fat Baby! Indeed, it is fatter than I imagined. 

![](featured.jpg "I made a poster!")

```c
//LED speaker

#include <Wire.h>
#include <Adafruit_NeoPixel.h>
#include <AltSoftSerial.h>    // Arduino build environment requires this
#include <wavTrigger.h>
#include <VL53L1X.h> 

VL53L1X sensor;
wavTrigger wTrig; 

// define NeoPixel Ring's pin and number of pixels
#define PIXEL_PIN 12  
#define NUM_PIXELS 48  

Adafruit_NeoPixel strip(NUM_PIXELS, PIXEL_PIN, NEO_GRBW + NEO_KHZ800);

// Pressure sensor
#define FSR_PIN1 A0
#define FSR_PIN2 A1
#define FSR_PIN3 A3

// PINK
const int R = 200;
const int G = 20;
const int B = 60;

int currentBrightness = 0;
int targetBrightness = 0;  

int sensor1Min = 0;
int sensor1Max = 0;
int sensor2Min = 0;
int sensor2Max = 0;
int sensor3Min = 0;
int sensor3Max = 0;

 // read pressure sensor (0 to 1023)
int fsrValue1 = analogRead(FSR_PIN1);
int fsrValue2 = analogRead(FSR_PIN2);
int fsrValue3 = analogRead(FSR_PIN3);

// distance
int distance = 0;               
unsigned long lastPlayback = 0; 
const unsigned long playbackInterval = 60000; 

unsigned long lastShortDistancePlayback = 0; 
const unsigned long shortDistanceDelay = 3000; 

unsigned long lastFSRActivation = 0;
const unsigned long fsrDebounceInterval = 2000; 

void neoRing1(){
  fsrValue1 = analogRead(FSR_PIN1);
  fsrValue2 = analogRead(FSR_PIN2);
  fsrValue3 = analogRead(FSR_PIN3);

  if (fsrValue1 > fsrValue2 && fsrValue1 > fsrValue3){
    targetBrightness = map(fsrValue1, sensor1Min+20, sensor1Max, 0, 255);
  }
  if (fsrValue2 > fsrValue1 && fsrValue2 > fsrValue3){
    targetBrightness = map(fsrValue2,sensor2Min+7, sensor2Max, 0, 255);
  }
  if (fsrValue3 > fsrValue1 && fsrValue3 > fsrValue2){
    targetBrightness = map(fsrValue3, sensor3Min+20, sensor3Max, 0, 255);
  }

  targetBrightness = constrain(targetBrightness, 0, 255);

 //  currentBrightness % targetBrightness
  if (currentBrightness < targetBrightness) {
    currentBrightness += 3; // increase
    //delay(10);
  } else if (currentBrightness > targetBrightness) {
    currentBrightness -= 3; // decrease
    //delay(10);
  }

  strip.setBrightness(currentBrightness);
  setRingColor(R, G, B);
  
  Serial.print("FSR Value1: ");
  Serial.print(fsrValue1);
  Serial.print("  FSR Value2: ");
  Serial.print(fsrValue2); 
  Serial.print("  FSR Value3: ");
  Serial.print(fsrValue3); 
  //Serial.print("targetBrightness: ");
  //Serial.println(targetBrightness);
  //Serial.print("currentBrightness: ");
  //Serial.println(currentBrightness);
  //Serial.println("ring done");
}


void setup() {
  Serial.begin(115200);   
  Wire.begin();
  Wire.setClock(400000);
  if (!sensor.init())
  {
    Serial.println("Failed to detect and initialize sensor!");
    while (1);
  }
  // information on range and timing limits.
  sensor.setDistanceMode(VL53L1X::Long);
  sensor.setMeasurementTimingBudget(50000);
  sensor.startContinuous(50);

  // Initialize NeoPixel
  strip.begin();
  strip.show(); 
  strip.setBrightness(20); 

  sensor1Min = analogRead(FSR_PIN1);
  sensor1Max = sensor1Min + 100;
  sensor2Min = analogRead(FSR_PIN2);
  sensor2Max = sensor2Min + 20;
  sensor3Min = analogRead(FSR_PIN3);
  sensor3Max = sensor3Min + 100;

  // Initialize WAV Trigger
  wTrig.start();
  delay(10);
  wTrig.stopAllTracks();
  wTrig.masterGain(10); // volume
  Serial.println("Setup complete");
}

void speaker(){
  distance = sensor.read() / 10; // mm --> cm
  if (sensor.timeoutOccurred()) {
    Serial.println("Distance sensor timeout!");
    return;
  }

  Serial.print("  Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  // Play different sound files
  if(distance <20){
    wTrig.trackPlaySolo(3); 
    delay(500);
    
  }
  if (distance <= 100 && distance >=20) {
    wTrig.trackPlaySolo(2); 
    //delay(2000);
  } else if (distance > 100) {
    if (millis() - lastPlayback >= playbackInterval) {
      wTrig.trackPlaySolo(4);
      lastPlayback = millis();
    }
  }

  //Pressure sensor
  //if (fsrValue1 > sensor1Min+50 || fsrValue2 > sensor2Min+10 || fsrValue3 > sensor3Min+50) {
    //Serial.println("All FSR sensors activated. Playing track 5.");
    //wTrig.trackPlaySolo(5); // 播放第 5 个音频
   // delay(500); // 防止重复触发
 // }
  if ((fsrValue1 > sensor1Min + 30 || fsrValue2 > sensor2Min + 5 || fsrValue3 > sensor3Min + 30) &&
      millis() - lastFSRActivation > fsrDebounceInterval) {
    Serial.println("All FSR sensors activated. Playing track 5.");
    wTrig.trackPlaySolo(5); 
    lastFSRActivation = millis(); 
  }
}

void loop() {
  neoRing1();
  speaker();
}

void setRingColor(int red, int green, int blue) {
  for (int i = 0; i < NUM_PIXELS; i++) {
    strip.setPixelColor(i, strip.Color(red, green, blue));
  }
  strip.show(); // refresh
}

void playFile(int trackNumber) {
  wTrig.stopAllTracks(); 
  delay(10); 
  wTrig.trackPlaySolo(trackNumber); 
  Serial.print("Playing track: ");
  Serial.println(trackNumber);
}
```

```c
#include <Wire.h>
#include <Servo.h>  
#include <VL53L1X.h>  

// 定义引脚
const int servoPin1 = 9;  
const int servoPin2 = 10; //（head）
const int servoPin3 = 11;
const int servoPin4 = 12;     
Servo myServo1;               // SG92R
Servo myServo2;      //MG995R（head） 
Servo myServo3;// SG92R
Servo myServo4;
VL53L1X sensor;

int currentAngle = 90;   
int targetAngle = 90;   

void servo(){

  int distance = measureDistance();
  if (distance < 0 || distance > 100) { 
    distance = 100; 
  }

  targetAngle = map(distance, 0, 100, 0, 180);
  targetAngle = constrain(targetAngle, 0, 180);

  if (currentAngle < targetAngle) {
    currentAngle += 3; 
  } else if (currentAngle > targetAngle) {
    currentAngle -= 3;
  }

  myServo1.write(currentAngle); //  1
  myServo2.write(currentAngle); //head
  myServo3.write(currentAngle); // 3
  myServo4.write(180 - currentAngle); // 2

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm, Current Angle: ");
  Serial.print(currentAngle);
  Serial.print(", Target Angle: ");
  Serial.println(targetAngle);
}

int measureDistance() {
  int distance = sensor.read() / 10; // mm --> cm
  if (sensor.timeoutOccurred()) {
    Serial.println("Sensor timeout!");
    return -1; 
  }
  return distance;
}


void setup() {
  Serial.begin(115200);   
  Wire1.begin();
  Wire1.setClock(400000); 

  sensor.setBus(&Wire1);
  sensor.setTimeout(500);
  if (!sensor.init()) {
    Serial.println("Failed to initialize sensor!");
    while (1);
  }
  sensor.setDistanceMode(VL53L1X::Long);
  sensor.setMeasurementTimingBudget(50000);
  sensor.startContinuous(50);

  myServo1.attach(servoPin1, 600, 2000);
  myServo1.write(currentAngle);
  myServo2.attach(servoPin2, 600, 2000);
  myServo2.write(currentAngle); 
  myServo3.attach(servoPin3, 600, 2000);
  myServo3.write(currentAngle); 
  myServo4.attach(servoPin4, 600, 2000);
  myServo4.write(currentAngle); 

}

void loop() {
  servo();

  delay(100);
}
```

More possibilities for improvement:

I hope to make a series of fluffy robots in the future, including robots of different sizes and shapes. With this experience, I will improve the form of interaction in the future. In addition, I will control the size and shape of the pressure sensor more accurately, and use more I2C sensors, such as capacitive touch sensor. At the same time, I will also pay attention to some operating techniques, I will better avoid short circuits, and debug faster.
