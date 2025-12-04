---
title: Final Project - Apology Jacket
date: 2025-12-04T12:18:00.000+02:00
authors:
  - Valerie Jeyeon YONG
image: final-img.jpg
showBgImage: false
---
# 
Apology Jacket


{{<youtube uYUhLdy16zA>}}



Apology Jacket is a wearable system that automatically apologizes whenever someone bumps into you. The jacket detects physical collisions on either shoulder and verbally apologizes. This simple interaction becomes a humorous yet unsettling encounter with automated social etiquette—an exploration of how easily mundane human behaviors can be handed over to machines.

In daily life, apologies happen quickly, instinctively, and often without real intention. In crowded spaces, you might say “sorry” repeatedly without even noticing. The gesture becomes almost mechanical. Apology Jacket takes this idea to the extreme. The project exaggerates and criticizes the automation of social etiquette and the absurd extension of AI assistance into even the smallest human gestures of politeness. We become more and more reliant on machines and AI, and now even to apologize. Apology Jacket exaggerates this dependency by outsourcing an intimate human behavior to an AI that reacts faster and more obediently than we ever could.

Technically, the jacket uses handmade pressure sensors sewn into the fabric at both shoulders. These sensors detect sudden physical contact and measure the intensity of the impact. A light bump triggers a short, simple apology. A stronger collision generates a longer, more dramatic apology. When a hit is detected, the jacket uses a Pico 2W to connect to Wi-Fi and request an appropriate apology sentence through the OpenAI API. The generated line is then spoken aloud using real-time speech API. 

During the few seconds the AI needs to generate a response, the jacket gently announces, *“One moment please, I’m thinking about the best way to apologize properly.”* This added delay injects humor and emphasizes the use of the machine.

In the process of creating this project, I learned how meaningful it is to construct something by hand that ultimately behaves in a highly automated way. Making the pressure sensors myself—cutting velostat, assembling layers of conductive fabric, sewing them directly into the lining of the jacket—required slow, tactile, physical work. This contrasted sharply with the instantaneous, immaterial nature of the AI’s response. The tension between manual craft and algorithmic automation became unexpectedly important: the “softness” and irregularity of the handmade sensors directly influenced how the jacket behaved. Even small stitching decisions affected sensitivity and detection patterns. The final outcome felt like a collaboration between the material quirks of the textile and the rigid logic of the code.

Ultimately, Apology Jacket imagines a future where AI reaches into the tiniest corners of social etiquette. If even our reflex apologies can be automated, what other aspects of interaction might be delegated next? The project is humorous, but it also carries a critical undercurrent. 

How much of our social behavior are we willing to automate, and what do we lose—emotionally, socially, or personally—when machines begin to speak for us?
