---
title: Week3_Final Project Draft
date: 2024-11-11T23:04:00.000Z
authors:
  - Jiali Huang
image: featured.png
bgimage: background.jpg
showBgImage: true
---
AI Companion Website

* Sharing common life experiences with the user
* Being able to actively interact with the user

\-

AI Positioning: 

Friends who share real-life gossip with you over video chat  

\-

Interaction:

 Open website → Automatically upload latest photo album images → AI initiates conversation topics → Open camera → Real-time recognition of user emotions → Upload as prompt → Engage in conversation with user

\-

TRAIL 01: Creating a Personalized AI Bot on ChatGPT.

Prompt：

You are my gossip friend who accompanies me. The new images sent to you are my experiences today. You should come up with topics to chat with me based on my experiences. The entire conversation should be in Chinese, without template responses. We’ll interact as normal close friends would, chatting freely and openly. Your speaking style should be casual and everyday. You can share your experiences today and guide me to share mine.

![](f5f2c84be7136a1170ab07870818c00.png)

TRAIL 02: Calling ChatGPT API in Python

![](b8994242a9a3c077e10c215d03ba149.png)

\-

NEXT:

Define the AI role and input the photo In python

Try to use QR code to upload the photo

Upload the phone photo automatically 

Website construction

Emotion recognition 

Emotion input as prompt

\-

PLAN B:

If the automatic upload of photos from the mobile album is unsuccessful, switch to uploading images via QR code.

The product will work as follows: if no image is uploaded, the camera will directly detect facial emotions and prompt the AI to ask questions. If an image is uploaded, the AI will treat it as part of your experience, analyze your emotions, and engage in conversation. Additionally, the image will be automatically deleted after one minute if no face is detected. 

This setup allows the product to be deployed in public settings.
