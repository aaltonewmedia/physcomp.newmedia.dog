---
title: Final Project
date: 2025-11-10T13:38:00.000+02:00
authors:
  - Valerie Jeyeon YONG
image: featured.jpg
showBgImage: false
---
# Final Project Ideas

## 1. Scentient Machine

#### Concept

Scentient Machine is a device that “reads the atmosphere” of a room. It interprets the invisible atmosphere of a space as emotional data.

Using MEMS gas sensors, the system reads the chemical composition of air, including human breath, perfume, humidity, and CO₂ levels, and translates these into emotional states.

ex) When the air feels heavy and warm, it displays: “The air smells tense.”

→ Tuto: [Building an Electronic Nose with MEMS Gas Detection Sensor](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269) 

[](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269)[](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269)

#### Emotion Labels and Training Conditions Examples

![](table.png)

→ Edge Impulse model for smell/'feeling of the air'

#### Installation Design

Like the interface of an air purifier or weather application. 

Digital display showing live emotional status (ex. Tense – 82% Confidence)

![](ref.png)

References

[Adnose, *Adnan Aga*](https://adnanaga.com/Adnose) - predict the smell of any object using AI

![](nose.jpeg)

[Smeller 2.0, *Wolfgang Georgsdorf -*](https://smeller.net/about/) deliver complex sequences of smells played in place of music notes. 
→ Maybe I can treats scent as a medium and transforms invisible chemical data into a performative art piece

![](smeller.jpg)

## **2. Apology Jacket**

#### Inspiration

I’m interested in the kind of simple and humorous interactive works that Matti showed us, which echo the spirit of Chindogu, a playful useless invention inventions born from everyday inconveniences.

{{<youtube rKhbUjVyKIc>}}

#### Concept

This project originates from an experience that everyone has: in crowded places, I often bump into people’s shoulders but can’t possibly apologize to everyone.

*Apology Jacket* is a wearable device, embedded in the shoulder of a jacket, that automatically apologizes whenever it detects physical contact. 
→ using AI TTS ?

The project explores the automation of social etiquette and the absurd extension of AI assistance into even the smallest human gestures of politeness. We become more and more reliant on machines and AI, and now even to apologize. *Apology Jacket* exaggerates this dependency by outsourcing an intimate human behavior to an AI that reacts faster and more obediently than we ever could. The result is both comical and unsettling: an endlessly polite jacket, apologizing to the world.

#### How it works

When the wearer bumps into someone, the embedded sensor detects the collision’s intensity. According to the force, the AI system generates and plays an apology with different tone and repetition. From a calm “Sorry” to an anxious stream of “I’m so sorry! Sorry! Sorry!”.

![](sketch.jpg)

<hr>

\*After reviewing and organizing my ideas, I find the use of an electric nose interesting, but I’m not very satisfied with the concept itself. So for now, I’m considering either simply running the **electric nose experiment** or moving forward with the **Apology Jacket.***

<hr>

# Apology Jacket Working Progress

I decided to choose Apology Jacket because I’m more confident in it (and it received more reactions from my classmates).

**Sensors**

* velostat
* conductive fabric
* small audio player



### First Test with Pressure Sensor

```

int pressureValue;
int thresholdValue = 600;
int thresholdValue2 = 900;

bool hasApologized = false;

void setup() {
  Serial.begin(9600);
}

void loop() {

  // Read pressure sensor (FSR or force sensor)
  pressureValue = analogRead(26);    

  // Collision detected 
  if (pressureValue > thresholdValue) {

    if (!hasApologized) { // only say sorry once per bump

      if (pressureValue < thresholdValue2) {
        Serial.println("Sorry");
      } 
      else if (pressureValue >= thresholdValue2) {
        Serial.println("SORRY SORRY!");
      }

      hasApologized = true;  // prevent repeating
    }
  }

  // RESET when pressure goes back to normal 
  else {
    hasApologized = false;
  }

  // Print readings
  // Serial.print("Pressure: ");
  // Serial.println(pressureValue);

  delay(10);
}
```

### 



[](https://www.hackster.io/giung-kim/how-to-use-openai-api-with-wizfi360-evb-pico-in-arduino-d10d5d)[](https://www.hackster.io/Shilleh/how-to-set-up-chatgpt-on-a-raspberry-pi-pico-w-5977bf)

[](https://www.hackster.io/Shilleh/how-to-set-up-chatgpt-on-a-raspberry-pi-pico-w-5977bf)
