---
title: Week2_Alt+Ctrl Interface
date: 2024-11-02T17:27:00.000Z
authors:
  - Jiali Huang
image: featured-1-.jpg
bgimage: background.png
showBgImage: true
---
I. Find an interesting existing Alt+Ctrl Interface

The Sword

![1](1.png)

* <https://www.youtube.com/watch?v=Kwi6uEYIEXU>

"The sword was born about 100 years ago. And long time, anyone could not pull out the sword. But, if make a Holy Grail that weighs the same as the artifact, can pull out the sword."

\-

\-[](https://www.youtube.com/watch?v=Kwi6uEYIEXU)[](https://www.youtube.com/watch?v=Kwi6uEYIEXU)[](https://www.youtube.com/watch?v=Kwi6uEYIEXU)

[](https://www.youtube.com/watch?v=Kwi6uEYIEXU)II. Come up with a concept for my own Alt+Ctrl Interface

1. Explore one sensor

Arduino Weighing Scale with Load Cell and HX711[](https://makersportal.com/blog/2019/5/12/arduino-weighing-scale-with-load-cell-and-hx711)[](https://makersportal.com/blog/2019/5/12/arduino-weighing-scale-with-load-cell-and-hx711)

![2](2.png)

* <https://makersportal.com/blog/2019/5/12/arduino-weighing-scale-with-load-cell-and-hx711>[](https://makersportal.com/blog/2019/5/12/arduino-weighing-scale-with-load-cell-and-hx711)

\-

\    2. Things that could be detected with the sensor and objects that could the sensor be attached to

This sensor can be connected to a small basin for weighing.

\-

\    3. Come up with a new game

Two key points of the AI Companionship Robot: 

* Sharing common life experiences with the user
* Being able to actively interact with the user

Design1: 

A robot positioned by the window or on the dining table that can use its camera to capture and recognize the user's home and window views. When it detects the user at the dining table, it proactively engages in conversations based on real-life data. Users can place objects in the small dish on the robot to express their psychological energy needs. The AI adjusts its personality traits based on the weight of the objects to provide appropriate companionship to the user.

(Placing objects into a box is also a collecting behavior that can have a soothing effect on one's mood. Psychologist Werner Muensterberg wrote in his book *Collecting: An Unruly Passion* that after experiencing a “separation, loss, or pain,” a person's desire to collect often becomes especially fervent. Each time a collector acquires a new item, they can fall into an illusion of being "all-powerful.")

Design2:

To detect the distance between the AI robot and the user, when the distance is very close (for example, around two meters), the AI robot will use its camera to search for the user. Once it recognizes the user's face, the robot will move toward the user.

\-

\    4. Sketches

![5](5.png)

![4](4.png)

Use TouchDesigner/Arduino/ChatGPT.

Use the MediaPipe component to recognize camera content and output prompts.

TouchDesigner accesses ChatGPT's API interface, sets up an input prompt window, and defines the robot's personality and dialogue scenario.

Arduino's infrared sensor/pressure sensor (triggered by sitting or stepping actions) detects a person.

The trigger value is inputted into TD, initiating the input of prompts to GPT to start the dialogue.

The Load Cell sensor inputs values to adjust GPT's personality.

\-

\-

III. Complete the MyCourses introductions for the 3D Printing and Laser Cutter workshops (DONE)

* <https://mycourses.aalto.fi/course/view.php?id=23273>
* [https://mycourses.aalto.fi/course/view.php?id=19552](https://mycourses.aalto.fi/course/view.php?id=23273)[](https://mycourses.aalto.fi/course/view.php?id=23273)
