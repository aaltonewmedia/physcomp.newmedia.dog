---
title: "Assignment 02: Waggle Waggle Revolution"
date: 2024-10-30T18:03:00.000Z
authors:
  - Fiona Keil
image: featured.jpg
bgimage: ""
showBgImage: true
---
# **1. Find an interesting Alt+Ctrl Interface**



I selected the game Roflpillar by the team Lucky Frame, which was submitted to GDC in 2014! 

The name caught my attention because it was a pun on ‘caterpillar’. It immediately made me picture the animal and how it moves, which I found inspiring. I wanted to create a funny insect-themed concept for this submission that makes participants do an unexpected/weird thing that is entertaining for themselves and observers, which is exactly what this project does, so it seemed like the perfect project to inspect more closely!

I didn’t just pick it just because it is insect-themed though, that is a bonus. I chose it more because I find the way of playing is very similar to how I want people to play my game at demo day so I feel like looking at this more closely can inspire me for my final project.



/\
/\
/

# **2. Come up with a concept for your own Alt-Ctrl Interface: Waggle Waggle Revolution!**



## Intro

For my final project in this course, I want to make an alternative controller for a game I make in Computational Art and design. It should be a cooperative game which makes participants do an unexpected/weird action to play to entertain both themselves and also spectators. I want the final project to be used in the future to recruit students into the DADA association and show what interesting things one can learn from New Media courses. For this small imaginative assignment I chose to ideate something for a similar purpose.

/



## Waggle Waggle Revolution 

I take care of a beehive on the university campus, so I decided to create a game concept to educate people about bees in a fun manner, which could be used at events and such. 

/

### Name:

The name is a pun on the game ‘Dance Dance Revolution’ in which you need to dance with other players to get points. This game does the same, except you need to waggle like a bee!



/

### Background:

In addition to pheromone signals, bees communicate through waggling their abdomens and performing a waggle dance, which follows a very specific dance-like pattern. With this movement, they communicate the location of things that are interesting to the hive, such as pollen sources. The bee that performs the most accurate and efficient waggle dance can convince the surrounding worker bees in the hive to investigate the food source/ new hive site they found. In studies researchers found that bees have varying skill levels in waggling and that sometimes better or more close ressources are ignored in favor of worse options simply because the bee who found it did not dance well enough. Bad dancers have been observed as being ignored by the hive more then 50 times due to their bad dance skills. This is an essential and fascinating aspect of bee life and rather funny, therefore making an alt+cntl game is a nice way to get participants interested in the topic of bees and how to protect them.



See the below image to understand their dance:





/

### Exact concept:

1. Two or more players put on a set of color-coded bee wings & abdomen costumes each after being instructed.
2. They stand in the game area which is marked with circles to guide the dancing.
3. They watch the instructional video on the game screen.
4. Round 1 begins on a 3,2,1 count, and the miniature meadow model lights up with colors showing the location of different food sources. This model includes a little hive model in the middle, which is the player's physical location, and an LED matrix underneath the model, which conveys the location of the food source each round.
5. Players have five seconds to interpret the model to figure out what path to waggle to their food source.
6. Players try to dance their pattern as fast and as correctly as they can according to their interpretation of the given information.
7. The judgment is based on how correct the information is, specifically the angle at which the participant waggled and how far they waggled to convey the distance. If there is a draw the faster waggler wins.
8. When they are done waggling, the screen shows the bees in the beehive turning to the best waggler and repeating their dance pattern, who is then shown as the winner! 



### Implementation:

I would like to elaborate more on the game screen, tutorial, and LED matrix model, but this assignment focuses specifically on sensors, so I will focus on this in the implementation section instead.



The information the game needs to huge the waggling consists of:

* The movement of players behind. If their butt doesn’t waggle as they dance the pattern, they are disqualified/ need to restart.
* Speed of the player's movement 
* The angle of movement (to indicate the angle to a food source)
* Tracking the location of players to ensure they walk the correct waggle pattern and also performing the pattern often enough to convey the distance.
* Usually, the location of the sun would also matter, but for simplicity's sake, it is permanently located at 12 o’clock on the model, and therefore the angle in real life reflects the one on the model and makes playing easier)

  /



### What sensor(s) to use to collect this information?

#### 1. Camera

A camera is installed in the center over every bee-player's marked area to track the speed and location of the player through code that interprets the visual data.



#### 2. Accelerometer

An accelerometer can be placed inside the ‘bee-wing and abdomen Alt+Ctrl devices’ to track whether players are actually shaking their behinds left to right often enough to qualify as waggling. This would require a Bluetooth or Wireless module to transfer the waggle data wirelessly to the computer running the game. Additionally, because there are two or maybe three players, each player would require their own Arduino, accelerometer, and Bluetooth module, which connect wirelessly to the game system.

/

## Conclusion

I love this idea. If it wasn’t inconvenient in the actual use case (outside in a field, next to beehives where we do our parties and introductions, so there is no place to hang cameras), I think this would be a viable game project. It would be best suited to a museum that teaches playfully about nature.
