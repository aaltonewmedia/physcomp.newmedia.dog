---
title: Project Hidden Under
date: 2025-12-10T14:23:00.000+02:00
authors:
  - Kuutamokuu
image: banner.png
bgimage: background.jpg
showBgImage: true
---
# **Hidden Under**

Hidden Under is an interactive art installation made for the project course called Physical Computing. This is the documentation of the project, which was proposed [earlier during the course](https://physcomp.newmedia.dog/2025/kuutamokuu/main-project-proposal-hidden-under/). This documentation is an overview of the project phases and a retrospective of why the design was executed this way and what would be done differently if there had been the needed resources to complete this work in its original design.

### Main aspects of the project

As illustrated in the [project proposal](https://physcomp.newmedia.dog/2025/kuutamokuu/main-project-proposal-hidden-under/), Hidden Under project aims to be an interactive art piece that rotates to reveal hidden illustrations from different layers of paintings. In essence, this project needs:

* Minimum of 2 rotating ‘canvases’ that have a transparent quality
* Rainstick interface(s) for the accelerometer(s)
* Motors to rotate the canvases
* A microcomputer controller(s) to rotate the motors
* A power source for the motors

#### **Canvases**

Acrylic sheets were chosen as a material for the paintings since they could be laser cut easily into shape and the sheet could have a transparent quality which was desired for the outcome. The idea was to create paintings that had holes that could be peeked through and paint that could be somewhat see-through. The project aims to examine and explore what kind of effects many somewhat transparent painting layers would look like when placed on top of each other and rotated.

This prototype project used only 2 layers of paintings. For a future version, the aim is to create bigger transparent paintings and more of them. To cut the desired size and shape of the sheets, some vector graphic work needed to be done for the laser cutting machine.

![](näyttökuva-2025-12-01-121314.png "The two motors used for this project")

For the two different motors that I had, I made a vector graphic version of the rotating motor piece to cut a suitable hole to enter the acrylic sheet directly to the rotating part. Once these were done and tested, I cut two round sheets with the centered holes with a laser cutting machine. The shapes were cut to make one bigger circle as the ground layer and a second one as the layer with the peeking holes.

![](tumblr_67f9790fd0c3b1ef7a17fc78ae1d1038_325facf0_1280.jpg "Acrylic sheet in laser cutter machine")

After this part was completed, I could proceed to the painting part.

#### **Painting**

I booked an atelier space to work with the acrylic sheets for some days. I had the impression that it was beneficial to coat the acrylic sheet with something like a pouring medium to make the acrylic paint stick better to the canvases. I tested the pouring medium on the transparent sheet, but noticed that it left a bit of a residue. I decided not to use it due to this. However, acrylic paint stuck to the acrylic sheets without an issue without any prework, so it was possible to proceed with the painting.

The paintings first got a background layer.

![](maalaus_1.jpg "Work in process with first layer of paint")

Then, additional details were added to the acrylic sheets. The aim was to use the bigger acrylic sheet as the background layer that would have the hidden objects painted on it. Hence, the bigger sheet was painted to be less transparent. The smaller sheet was left with the window holes to peek through. The smaller sheet also was purposefully painted to be more transparent so that the colors of the upper sheet(smaller) would blend more with the lower sheet(bigger).

![](maalaus_2.jpg "The paintings got more details")

The last part of the painting process was to paint the illustrative objects that would be revealed through the smaller sheet in right angles. The objects and holes were positioned so that only one of the hidden objects would be revealed at the time in the correct angle. There is a total of 7 holes and each hole reveals one hidden illustration.

The hidden illustrations are:

* A mouse
* Silver coins
* Gold necklace
* Prehistoric pot
* A boat-shaped hammer head
* Deer skull
* Plastic toy

![](maalaus_3.1.jpg "Painted illustrations on the bigger sheet")

As a finish, the paintings were sprayed with fixative to set the paint into the sheets and prevent it coming off from the sheets. Once this phase was done, it was time to fit these paintings together with the motors.

#### **Rainsticks**

Rainsticks were chosen as the controlled interface for the project to rotate the paintings since rainsticks as an instrument give audio feedback as tilted. The original idea is to create a soothing, sensory experience of gentle rainstick sounds and calm rotating earthy visuals. There were hopes to create the rainstick with more authentic wood material and sound, however, within the project timeframe the rainsticks were created with a more straightforward DIY way.

The rainsticks were made of two different sized cardboard cylinders. To create a more rainlike sound with the cylinder, an additional filling piece was added inside the cylinder. These were created with grillsticks and a piece of jute rope. The grill stick parts were hot glued to the jute rope and then pulled inside the cylinder, before adding the rainstick filling.

![](tube.jpg "Filled rainstick tube")

I used rice as the filling for these rainsticks to make the rain sound. In the bigger cylinder I additionally added some pistachio shells. The cylinder tubes were then sealed with paper and tape. These cylinders were painted to match the general earthy aesthetic as the paintings.

Since the sensor couldn’t fit inside the rainstick with the additional wooden pieces, the sensor needed a pocket to fit outside of the stick. A pouch was made outside the stick with tape and then painted.

![](sensoritasku.jpg "The rainstick was sealed and painted")

#### **Electrical setup**

Before the painting process, different setups were tested and planned for the sensors. The first idea was to use one RaspberryPi minicomputer to control the two sensors that could read data from tilting. This idea was scrapped due to the need for additional parts. There were many RaspberryPi’s to use, so instead of one, two were used.

The RaspberryPi’s were setup as following:

![](controllersetup.png "Illustration of the parts attached to RaspberryPi")

The RaspberryPi's were attached to a Prototyping PiCowbell part. Prototyping PiCowbell is the part that can read the specific type of cable for the sensors used for the rainsticks. This part was attached to the Kitronik Pico Motor Driver Board part. This board receives the needed power voltage to use the motors. This part is connected to the motors directly.

Both of the RaspberryPi's used for this project were individually programmed to work with the different motors I had. These motors were introduced earlier in this documentation. Once the accelerometer sensor and motor were calibrated to move slowly and smoothly enough within the accelerometer data values, they were set up to use the same battery power source to move.

To summarize, this illustration shows how the electrical setup was managed:

![](electricalsetup.png "Illustration of the organized electrical parts")

To explain the illustration simply:

(7.5V) Power source has a part that distributes energy ( + & - ). To each Motor Driver board, a + and - wire is attached to give the RaspberryPi and the accelerometer power. The Motor is connected to the Motor Driver board directly. The Prototyping PiCowbell has an accelerometer attached which give out tilting data. The RaspberryPi controls the logic of the received accelerometer data and powers the motor speed according to the logic.

#### **Programming**

The code includes libraries needed to read the sensor value. The accelerometer sensors are Adafruit_MSA311 and Adafruit_MSA301. 

The variables that are included at the beginning are the float values of the accelerometer data. The Accelerometer gives data on x, y and z axis. This project uses only the y axis values, which is the axis that is tilted with the rainstick.

The variable integer values are M1_C1 and M1_C2, which are the pin holes in the Motor Driver board.

The integer speed is the variable that controls the motor speed according to the tilting values.

In the setup, there is code that is used to help fix bugs that might be encountered from the accelerometer. Then, the motor pins are assigned to be outputs.

In the loop of the code, the accelerometer reads the data values that it is gathering from the tilting. Then, the data from the y axis is mapped so that the accelerometer values can be translated to the motor speed value.

Then, to put it simple, the accelerometer mapped value rotates the motor in different speed depending on the threshold. If the accelerometer tilting value is not enough (not enough tilted), the motor does not move.

At the end, there are three functions. These ones tell the motor what way they are supposed to rotate or if they are supposed to stay still if they are called during the earlier step.

Basically, what this code does is rotate the two canvases to two different direction depending on what way the rainstick is tilted.

```
// add all necessary libraries
#include <Wire.h>
#include <Adafruit_MSA301.h>
#include <Adafruit_Sensor.h>

//Initial speed of the motor, map later to float of the X axis of accelometer1
// int speed = 200;

// Comment/Uncomment as needed for specific MSA being used:
Adafruit_MSA311 msa;
//Adafruit_MSA301 msa;

//--------------------ACCELOMETER------------------

float x,y,z; //Accelometer axis

//----------------------MOTOR-----------------------

// pins for MOTOR 1
int M1_C1 = 2;
int M1_C2 = 3;

// speed of the motors 0-255 Note: Unsure if this motor also uses this range, I couldnt find from the internet
int speed;


void setup() {

  // put your setup code here, to run once:

  //------------ACCELOMETER-----------

  Serial.begin(115200);
  if (! msa.begin()) {
        Serial.println("Failed to find MSA301/311 chip");
        while (1) { delay(10); }
    }
  Serial.println("MSA301/311 Found!");


  //------------MOTOR 1 PIN SETUP-----------

  // change the pins to outputs
  pinMode(M1_C1, OUTPUT);
  pinMode(M1_C2, OUTPUT);


}

void loop() {
  // put your main code here, to run repeatedly:
    // get X Y and Z data at once
    msa.read(); 
    
    x = msa.x;
    y = msa.y;
    z = msa.z;

    // print the values
    Serial.print(x);
    Serial.print(",");
    Serial.print(y);
    Serial.print(",");
    Serial.println(z);
    delay(50);

    //----------MAPPING ACCELOMETER TO MOTOR-----

    // Mapping the accelometer value to the range of the motor = accelometer range 0-2000 -> motor goes from 0 speed to 255
    speed = map(abs(y), 500, 2000, 100, 150);

      //If x shows positive values, go forward

      if (y >= 500)
      {
        motorOneForward();
      }

      //If x shows negative values, go backward

      if (y <= -700)
      {
        motorOneBackward();
      }

      //If the motor is at "resting" stage (not tilted), stop motor

      if (y >= -700 && y <= 500){
        motorStop();
      }

  }
  

//---------------------MOTOR CONTROLS-------------------

void motorOneForward() {
  analogWrite(M1_C1, speed);
  analogWrite(M1_C2, 0);
}

void motorOneBackward(){
  analogWrite(M1_C1, 0);
  analogWrite(M1_C2, speed);
}


void motorStop(){
  analogWrite(M1_C1, 0);
  analogWrite(M1_C2, 0);
}
```

```
// add all necessary libraries
#include <Wire.h>
#include <Adafruit_MSA301.h>
#include <Adafruit_Sensor.h>

// Comment/Uncomment as needed for specific MSA being used:
Adafruit_MSA311 msa;
//Adafruit_MSA301 msa;

//--------------------ACCELOMETER------------------

float x,y,z; //Accelometer axis

//----------------------MOTOR-----------------------

// pins for MOTOR 1
int M1_C1 = 2;
int M1_C2 = 3;

// speed of the motors 0-255 Note: Idk if this motor also uses this range, I couldnt find from the internet
int speed;


void setup() {

  // put your setup code here, to run once:

  //------------ACCELOMETER-----------

  Serial.begin(115200);
  if (! msa.begin()) {
        Serial.println("Failed to find MSA301/311 chip");
        while (1) { delay(10); }
    }
  Serial.println("MSA301/311 Found!");


  //------------MOTOR 1 PIN SETUP-----------

  // change the pins to outputs
  pinMode(M1_C1, OUTPUT);
  pinMode(M1_C2, OUTPUT);


}

void loop() {
  // put your main code here, to run repeatedly:
    // get X Y and Z data at once
    msa.read(); 
    
    x = msa.x;
    y = msa.y;
    z = msa.z;

    // print the values
    Serial.print(x);
    Serial.print(",");
    Serial.print(y);
    Serial.print(",");
    Serial.println(z);
    delay(50);

    //----------MAPPING ACCELOMETER TO MOTOR-----

    // Mapping the accelometer value to the range of the motor = accelometer range 0-2000 -> motor goes from 0 speed to 255 etc
    speed = map(abs(y), 500, 2000, 100, 150);

      //If x shows positive values, go forward

      if (y >= 500)
      {
        motorOneForward();
      }

      //If x shows negative values, go backward

      if (y <= -700)
      {
        motorOneBackward();
      }

      //The treshold to stop motor

      if (y >= -700 && y <= 500){
        motorStop();
      }

  }
  



//---------------------MOTOR CONTROLS-------------------

void motorOneForward() {
  analogWrite(M1_C1, speed);
  analogWrite(M1_C2, 0);
}

void motorOneBackward(){
  analogWrite(M1_C1, 0);
  analogWrite(M1_C2, speed);
}


void motorStop(){
  analogWrite(M1_C1, 0);
  analogWrite(M1_C2, 0);
}
```

The codes are generally different for each RaspberryPi due to the sensors being both different model. One of the sensor gave out some odd values, so they were mapped according to the values that were given. (Typically one of these accelerometers give values from +2000 to -2000, but one of the sensors were giving something like +2200 to -1800)

#### Final installation:

![](pxl_20251208_140915294.jpg "Hidden under installation setup for demoday")

##### Final video:


 {{<youtube tdc5WGjdZOk>}}
