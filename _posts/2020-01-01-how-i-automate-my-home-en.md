---
layout: post
title: How I automate my home
tags: [IoT]
comment: true
---

This blog describes the automations I use in my home. Most of the automations
here assume specific life styles, but I hope the content be inspiring to people
who want to build their own smart homes.

For details on the IoT devices I'm using, please check
[IoT Devices in my home](https://japaniot.github.io/2020/01/01/my-iot-devices-en/).

## Platform

I'm stuck to iPhone, so I use HomeKit to automate my home. Home Assistant would
be a much better choice if you use Android phones.

There are not much HomeKit devices in the market, especially in Japan, so I
write [HomeBridge](https://homebridge.io) plugins for almost every device in my
home.

#### HomeKit programming

Apple does not provide much scripting support for HomeKit other than very simple
"if then" automations, I use the [Controller for HomeKit](https://apps.apple.com/us/app/controller-for-homekit/id1198176727)
iOS app for editing HomeKit automations, which is free and can add various
conditions to "if then" automations. With some HomeBridge plugins it should be
possible to do any kind of automation.

## Automations

Here are the automations in my home, rules are written in pseudocode and can be
easily translated to any home automation script.

I have omitted basic use cases like having Siri close the bedroom light or
closing all lights when leaving home.

### Theater mode

I use projector to watch videos, which is very weak to sunshine. So I have
installed electric blackout curtains in the living room, and use scenes to turn
off the curtains when watching videos.

Currently I have 4 scenes:

1. "Lunch time"
  - Turn on projector
  - Set the curtain aside dinning table to 50%
  - Set all other curtains to 100%
2. "Lunch over"
  - Turn off projector
  - Set all curtains to 0%
3. "Dinner time"
  - Turn on projector
  - Turn on the lights above dinning table
  - Set all curtains to 100%
4. "Dinner over"
  - Turn off projector
  - Turn on all lights in living room

This is how the "dinner time" scene looks like ⬇️
<iframe width="560" height="315" src="https://www.youtube.com/embed/pmf46YsNnLQ?rel=0&controls=0&showinfo=0&loop=1&playlist=pmf46YsNnLQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Volume adjustment depending on video inputs

For the projector, I use Apple TV for watching videos, and Nintendo Switch for
playing games. Their volume levels are quite different, normal volume for videos
could be too loud for playing games.

Luckily my projector runs webOS and provides APIs for checking the video input,
so I use the [homebridge-webos-tv](https://github.com/merdok/homebridge-webos-tv)
plugin to automatically change volume depending on the video input.

Required devices:

* Smart TV/projector

Rules:

```
when [projector.hdmi.1] is "on" do
  set [projector.volume] to "60%"
end

when [projector.hdmi.2] is "on" do
  set [projector.volume] to "40%"
end
```

### Closet auto-light

I have a walk-in closet room and a storage room in my home, they are small don't
have any window, so I programmed them to turn on light when door is opened and
turn off light when door is closed ⬇️.

<iframe width="253" height="450" src="https://www.youtube.com/embed/2ma39f8irOc?rel=0&controls=0&showinfo=0&loop=1&playlist=2ma39f8irOc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Required devices:

* Smart light bulb/switch
* Door sensor

Rules:

```
when [door sensor] is "open" do
  set [light] to "open"
end

when [door sensor] is "close" do
  set [light] to "close"
end
```

### Automatic curtain in bedroom

I have installed electric blackout curtains in my bedroom and use sunshine to
wake me up.

I make curtain leave a small notch in the night so my body can get used to
sunshine gradually in the morning, and then on the alarm time the curtain would
be half-opened and I would wake up in a more nature way.

Depending on how you want to wake up, you can also turn off the curtain
completely in the night and then open it to 100% on wakeup time, it would work
better than any alarm.

Required devices:

* Electric curtain

Rules:

```
when [time] is "20:00" do
  set [curtain] to "10%"
end

when [time] is "7:30" do
  set [curtain] to "50%"
end

when [time] is "9:00" do
  set [curtain] to "100%"
end
```

### Open light for Dyson sweeping robot

I use the Dyson 360 Heurist sweeping robot in my home, it relies on camera to
scan the room so it does not work fine in dark rooms. I have added rules to turn
on lights when the robot starts to run, and turn off lights when it is back to
dock.

The biggest challenge is how to connect it to home automations, I ended up
writing [my own homebridge plugin](https://github.com/japaniot/homebridge-dyson-robot).

Required devices:

* Dyson 360 sweeping robot
* Smart light switches

Rules:

```
when [dyson] is "running" do
  set [light] in [living room] to "open"
  set [light] in [bedroom] to "open"
  ...
end

when [dyson] is "docked" do
  set [light] in [living room] to "close"
  set [light] in [bedroom] to "close"
  ...
end
```

### Auto-close light

As with most Japanese homes, the corridor of my home does not have much
sunshine. I pass through it very frequently don't need to turn on lights for
most times, but in the end when I had to turn on lights I often forgot to turn
them off.

So I set up an automation to turn off the lights of corridor 5 minutes after
they were opened.

Note that the default timeout option of HomeKit automation is basically useless,
I use the [homebridge-delayed-switches](https://github.com/grover/homebridge-delayed-switches)
plugin to create delayed switches which can be used as timers.

Required devices:

* Smart light switches

Rules:

```
let [countdown switch] = new DelayedSwitch("close after 5 minutes")

when [light] is "open" do
  set [countdown switch] to "open"
end

when [countdown switch] is "close" do
  set [light] to "close"
end
```

### Kitchen auto-light

I usually spend the night in rooms other than the living room, and sometimes I
need to get into the dark living room to get some food or water. So I make the
lights in kitchen automatically turn on/off when people enter/leave the living
room ⬇️.

<iframe width="253" height="450" src="https://www.youtube.com/embed/y5lf4_psIns?rel=0&controls=0&showinfo=0&loop=1&playlist=y5lf4_psIns" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

(The kitchen and dinning area in my home are all in the living room, and I have
smart switches to control each area's lights.)

There are a few things that make the rules a bit complicated:

1. This automation should only work in night.
2. This automation should not work when people are staying in the living room
   like eating or watching TV.
3. After entering the room I may want to stay in the room and stop this
   automation.

In the end I use the state of TV and lights in the living room to indicate
whether people are staying in the room.

Required devices:

* Smart light switches
* Smart TV
* Occupancy sensor

Rules:

```
when [occupancy sensor] is "occupied" do
  if [time] is "night" and
     [tv] is "off" and
     [light] in [living room] is "off" and
     [light] in [dinning room] is "off" then
    set [light] in [kitchen] to "on"
  end
end

when [occupancy sensor] is "empty" do
  if [time] is "night" and
     [tv] is "off" and
     [light] in [living room] is "off" and
     [light] in [dinning room] is "off" then
    set [light] in [kitchen] to "off"
  end
end
```

#### More on occupancy sensor

Currently most occupancy sensors are essentially motion sensors, so if I stay in
the room for relative long time without much movement the sensors could think no
one is in the room and then turn off the lights.

As the automation would stop if other lights in living room are on, I would turn
on another light in living room if I decide to stay in it for a while. It takes
some time to get used to but works pretty well for me.

Also if your living room is large enough only one motion sensor may not be
enough to cover all areas, while adding rules for multiple motion sensors would
be a disaster.

I have been using the [homebridge-occupancy-delay](https://github.com/archanglmr/homebridge-occupancy-delay)
plugin to connect multiple motion sensors together into one virtual occupancy
sensor.

### Toilet auto-light

In Japan, toilet is usually in a separate small dark room, so to use the
toilet I have to turn on/off the light every time. I have an automation to
automatically turn on/off the light when entering/leaving the toilet room ⬇️.

<iframe width="253" height="450" src="https://www.youtube.com/embed/TbcTh4-FqSs?rel=0&controls=0&showinfo=0&loop=1&playlist=TbcTh4-FqSs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I also made the automation change the brightness of the light base on current
time, so when people enter the toilet in midnight, the light would be very dim.

There are actually lots of automatic toilet light switches based human sensor,
however if you sit on the toilet for a long time without much movement, the
light would turn off while you are still in the room. For the same reason I do
not use motion sensor to implement this automation.

What I do is using door as occupancy indicator: the first time people opens the
door the light would turn on, the second time people opens the door the light
would turn off. (This however does not apply to areas outside Japan, where
toilets are usually in the bathroom instead of a separate room.)

When implementing this automation, I found that with HomeKit if I use a switch
in both conditons and outputs, automations would be completely out of order. So
I use the [homebridge-dummy](https://github.com/nfarina/homebridge-dummy) plugin
to create several dummy switches as state indicators, the principle is: the
switch should only be in either conditions or outputs, and must not be in both.

I also found that HomeKit can not set time range to midnight like "10pm - 5am",
so I wrote [my own homebridge plugin](https://github.com/japaniot/homebridge-time-range)
instead.

Required devices:

* Smart light bulb/switch
* Door sensor

Rules:

```
let [midnight] = new TimeRangeSwitch("10pm - 5am")
let [occupied] = new DummySwitch("off")
let [leaving] = new DummySwitch("off")

when [door sensor] is "open" do
  if [occupied] is "off" and [midnight] is "off" then
    set [light] to "100%"
    set [leaving] to "off"
  end
end

when [door sensor] is "open" do
  if [occupied] is "off" and [midnight] is "on" then
    set [light] to "10%"
    set [leaving] to "off"
  end
end

when [door sensor] is "close" do
  if [light] is "on" and [leaving] is "off" then
    set [occupied] to "on"
  end
end

when [door sensor] is "open" do
  if [occupied] is "on" then
    set [light] to "off"
    set [leaving] to "on"
  end
end

when [door sensor] is "close" do
  if [leaving] is "on" then
    set [occupied] to "off"
  end
end
```
