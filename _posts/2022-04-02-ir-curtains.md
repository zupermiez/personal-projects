---
title: "IR controlled curtains"
date: 2022-04-02 10:00:00 +0200
categories: [arduino, IR]
tags: [arduino, IR]
image: /assets/img/ircurtain.jpg
---

## IR controlled curtains

In this fairly simple project I built remote controlled blackout curtains into my bedroom.
I built these by using an ESP-32 and an IR-receiver. For the motor i used a simple 5v dc-motor leftover from a toy.
I 3d printed custom gear. For the IR Remote i used my Logitech speaker remote.

## Learning
This was my first time doing IR-sensors or requiring custom 3d printed gears. The gears had have ball shaped holes of certain size and distance apart. However I found online a ball gear generator. I tried, but I wasnt able to capture specific IR-signals from the remote, so I had to settle for a simple single button solution.

## Components 
- ESP-32
- motor controller shield
- IR receiver & remote
- Button
- Motor
- 3d printed parts


![Ball gear](/assets/img/ballgear.jpg)


## Demo videos

<p float="left">
  {% include embed/youtube.html id='tfPkrqV8gG8' %}
  {% include embed/youtube.html id='hrTNpXOnDdg' %}
</p>
