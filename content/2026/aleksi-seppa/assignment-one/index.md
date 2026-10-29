---
title: Assignment one
date: 2026-10-29T11:09:00Z
showBgImage: false
showComments: true
heroStyle: background
draft: false
---
Trying to get this setup to work was a bit toughytough. I had forgotten all that I knew from working with Arduino. I still don't fully understand how my code works but I tried to make //quatations everywhere so I would get the logic. But usually in school I feel like I don't understand anything but eventually I will.



![](/images/2026/aleksi-seppa/mvtcbmll-yvpdyet3.jpg)

  
  
In the process I got stuck multiple times because of wrong wiring: I thought I didn't need the 3.3v connection at all, I thought there would be current running from different pins... I don't know why I thought that even though we always use 3.3v. Then the variables and if commands are still a bit like wtf. I tried to do it by myself and struggled for a long time, then I asked Ai to teach me and not to give me full solution and it was helpful, but after long time of struggling I eventually checked the solution. I think I need to study more the Arduino reference page to see what is possible etc.  
  
Here is the code that got it working (I think it still is a bit buggy if u press it really fast):

```

int vaihe = 0;
int edellinen = HIGH;

void setup() {
  // LEDit
  pinMode(15, OUTPUT);
  pinMode(14, OUTPUT);

  // Nappi
  pinMode(16, INPUT);

  Serial.begin(9600);

  // Molemmat LEDit aluksi pois
  analogWrite(15, 0);
  analogWrite(14, 0);
}

void loop() {

  int btn = digitalRead(16);
  Serial.println(btn);

  // Tarkistetaan painettiinko nappia juuri nyt
  if (btn == LOW && edellinen == HIGH) {

    vaihe = vaihe + 1;

    // Kolmannella painalluksella takaisin alkuun
    if (vaihe == 3) {
      vaihe = 0;
    }
  }

  // LEDien ohjaus
  if (vaihe == 0) {
    analogWrite(15, 0);
    analogWrite(14, 0);
  }

  else if (vaihe == 1) {
    analogWrite(15, 255);
    analogWrite(14, 0);
  }

  else if (vaihe == 2) {
    analogWrite(15, 255);
    analogWrite(14, 255);
  }

  // Muistetaan napin tila seuraavaa kierrosta varten
  edellinen = btn;


}

```

