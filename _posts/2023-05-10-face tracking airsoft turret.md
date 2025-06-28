---
title: "Face tracking airsoft turret"
date: 2023-05-10 10:00:00 +0200
categories: [computer vision, gun]
tags: [sample, demo]
image: /assets/img/turretcover.jpg  # Add cover image

---

## Face tracking airsoft turret

 I built an airsoft turret that had a camera using a raspberry pi 4 model b, 2 stepper motors and an old USB webcam. The turret had two control modes. You could either drive it using a keyboard or then use the cameras to do face tracking. I got the proof of concept working, but the turret was definitely mediocre due to parts. These were parts I had originally acquired for previous projects such as the ice fishing robot and the scale of waste system.




 ![vaaka](/assets/img/turretfacetrack.jpg){: width="400" style="float: left;"}

 *Here is what the camera is seeing*



## Technical details / challenges
There was one Nema 17 stepper motor for horizontal rotation and one for vertical rotation. The control code written in python ran on a raspberry pi 4 model b. Unsurprisingly my hardware presented some limitations. The face tracking was not too fast running on a raspberry pi but I was happy that I got the proof of concept working. I used opencv for the face tracking. Some challenges were also presented by the stepper motors weak lifting strength in the vertical direction. As always some custom 3d printed mounts and parts needed to be designed and printed. To trigger the gun I disassembled it and attached a general relay module to where the trigger switch used to be. One of the biggest challenges was to handle the recoil from the gun. I didnt have any mechanisms for this so as can be seen from the second demo video, once the gun starts firing the recoil shoots the barrel upwards almost immediately.

## Control
The face tracking control algorithm was incredibly simple since I had a camera mounted to the barrel of the gun. "If the x-coordinates of the middle point of target face are greater than the x-coordinates of the frames middle point, move left." And similar for the y-direction. Unfortunately I dont have the original code available anymore.




## Demonstration of the face tracking 
{% include embed/youtube.html id='hG84c8V_Np0' width="250" %}


## Another demo including shooting the gun
{% include embed/youtube.html id='fNK9Q4p9goY' width="250" %}





