---
title: Alt.Ctrl
date: 2024-11-18T14:34:00.000Z
authors:
  - Sasha Tikachev
image: featured.jpg
showBgImage: false
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/M_3Daovh74A?si=llcfRzBccUwSGE1C&amp;start=1234" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Retrieved from:
[_🔨📺Beyond Screens - ALT+CTRL show📺🔨_](https://www.youtube.com/watch?v=M_3Daovh74A&t=1249s)
The ALT+CTRL show is created and hosted by game designers and artists:
- 🛠️🕹️Tatiana Vilela dos Santos🕹️🛠️
    - [https://mechbird.fr](https://mechbird.fr/)
    - Twitter: @mechbird
    - Instagram: @tatiana_vilela_
    - [https://tatianavileladossantos.com](https://tatianavileladossantos.com/)
- 🎭🕹️Alistair Aitcheson🕹️🎭
    - [https://alistairaitcheson.com](https://alistairaitcheson.com/)
    - Twitter: @agaitcheson
    - Instagram: @agaitcheson
    - Twitch: @agaitcheson
# Cartload O'Fun

- [Helen Kwok](https://helenkwok.net/)
- [Chad Torpak](https://mr-chad.com/)

## Short description:
The game is set in the public transport. Two players are using the holding bars as controllers. They control one character, with one player making the character move left and right by squeezing and releasing the bar, while the other player makes the character go up and down. Strangers around are invited to participate.

More info:
- https://mr-chad.com/cart-load-o-fun/
- [http://exertiongameslab.org/projects/cart-load-o-fun](http://exertiongameslab.org/projects/cart-load-o-fun)
- [http://cusp-design.com/read-cart-load-o-fun-july-6-2013](http://cusp-design.com/read-cart-load-o-fun-july-6-2013)
- [https://newscientist.com/article/mg21829185-600-play-your-way-to-work-with-interactive-games](https://newscientist.com/article/mg21829185-600-play-your-way-to-work-with-interactive-games)

## Thoughts

The decision to choose this game was cause i really liked the thinking process in the interview. The project is from 12 years ago, and the details of it helps put ctrl.alt games into perspective. 
But, of course, such Playful(!) usage of the public space, which is thought here also as [liminal](https://en.wikipedia.org/wiki/Liminal_space_(aesthetic)), is what made it stand out for me.
.
One aspect is the experimentation with the audience, without knowing the outcome, or the even the fact of participation. I think that it was very mindful to make it in a way that people can first observe, not forcing everyone into the game. At the same time designing it to be accessible in a way to make the controls, so that it easy to jump in - using familiar surface and gestures.

In the interview, they also talk about one great interaction that happened - a person who initially said "I don't play games" decides to play and how is whole attitude changes in the process.

Would be interested to know, what would be reaction to this kind of game today. Would that be different outcome?

# 2. Own Concept - yoyo fight/dance/rythm game

The idea came up from thinking of various hand gestures from personal experience. I was playing yoyo for some time as a kid, and the gesture naturally came up as quite unique one. 
The moment when yoyo is down is interesting - the speed and acceleration (especially twisting) is radically (exponentially) changing to the zero and then back.

![pos and vel](https://www.labtrek.it/torzo/yo-yo.html/Image2.gif)

[FIGURE 2. Position and velocity vs. time of a yo-yo.](https://www.labtrek.it/torzo/yo-yo.html/Torzo.html)

I was thinking then how that can be used, when you need the precision. 
SImilarly i was thinking that there is another value - distance. 
So i thought of what if the players are playing some mortal combat/boxing game - one needs to hit fast and precise. the screen can be used or even the some physical mechanism. 
Then horse racing idea was there - but the idea of hitting the horse in the right moments and precision was off putting.
Other idea, controlling a character in some sort of running rythm game, where you need to jump in the right time.

As of sensors, I liked this simplee idea for velocity from this reddit post:

> a single hall effect sensor, you're only able to detect that there's a magnet. You need two to be able to determine wheel rotation direction.
> 
> Your time between pulses determines the period, if you need angular velocity in radians per second, you just divide 2 pi by your period (in seconds).
> 
> So if the wheel is spinning at 2 revolutions per second (angular velocity: 4pi/sec), you'll get a pulse rising edge every 500ms: `2pi / 0.5 = 4pi`

![sideways hall effect sensing](https://www.electronics-tutorials.ws/wp-content/uploads/2018/05/electromagnetism-mag31.gif)

That would require accomodation of arduino on the yoyo itself, for communication. 

Other option could be using external lidar, but that would limit the portability - if everything is measured by the yoyo - it would be possible to play in the public areas as in the example from Cartload O'Fun.

(i would want to here sketches of the whole concept, which I will do still later..)
