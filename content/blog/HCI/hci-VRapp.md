---
title: "HCI - VR app"
date: 2023-10-31T13:45:06+06:00
image: images/blog/HCI/vr-female.jpg
feature_image: images/blog/HCI/vr-female.jpg
author: Pauline SPINGA
subject : "HCI"
---

Today's article is about create a VR game using the roll a ball game
![Roll a ball game](https://i.imgur.com/lgouSlH.mp4)

The goal is to be able to carry the board and move the board in order to pickup collectibles.

In order to do that I created a scene, to simulate an environment for the player. Tje game is placed on the table. 
![scene](https://i.imgur.com/1IL88C0.png)

It was my very first VR application and the most difficult part was to manage how the player could grab the platform in order to play. I had to correctly configure joysticks and define an AttachPoint on my playboard. 

Some keys elements that are important : 

* XR Interaction Manager
* Locomotion system
* XR origin
* Attach Point

Here is a demo of what the player can see and do: 
![demo](https://i.imgur.com/37us3Rt.mp4)
