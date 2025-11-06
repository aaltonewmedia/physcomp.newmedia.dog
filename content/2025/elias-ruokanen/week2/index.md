---
title: Week2
date: 2025-11-06T02:36:00.000+02:00
authors:
  - Elias Ruokanen
image: img_0037.png
showBgImage: false
---
https://shakethatbutton.com/bib-goes-home-2/

Bib goes home uses projection mapping to turn a popup book into an interactive game/book/movie.

What caught my eye was the aesthetics, the rugged and sketchy feel of the cardboard mixed with bright projection mapped visuals works really well - like the mind’s eye / imagination overlayed on the mundane background. It feels like a cohesive product, where every element serves the whole. Not necessarily groundbreaking, but it doesn’t need to be.

GAME IDEA = guess the object

Have two matrixes (if budget is not an issue, say each matrix is 100x100)  of distance sensors. They are perpendicular to each other. They form an invisible box, where distance 0 is one edge and 1000 another. In this box is a 3D field of points where the rays from sensors intersect (or if that a problem, they are at slightly different heights).

3d objects can be mapped to this space, with every vertex taking one point. One can then map touching this object by when one’s hand crosses a point. This touching can be signalled by audio, or blinking leds, or anything else. When one’s hand crosses out of the boundary of the object the signal stops. One can trace the contour of the object with a finger or stick. Thus one must use other sensory cues as stand-ins for touching. 

You could have two boxes with one user making a sign with their hands and the other trying to match it (how do you scale for different hand sizes though… hmm). Or record motions and match that. Or try to catch a moving ball inside the box. Or maybe get two people to shake hands (might be a fun interaction, with people telling each other to stop moving etc.)
