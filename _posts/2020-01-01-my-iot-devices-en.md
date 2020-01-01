---
layout: post
title: IoT Devices in my home
tags: [IoT]
comment: true
---

Most devices discussed bellow are only being sold in Japan, but it should be
possible to find similar products in your country.

For examples on how to set up home automations, please check
[How I automate my home](https://japaniot.github.io/2020/01/01/how-i-automate-my-home-en/).

## Door sensor and motion sensor

I use [Aqara](https://www.aqara.com) products for door and motion sensors.

<img src="https://user-images.githubusercontent.com/639601/71551583-d8f43780-2a2d-11ea-8dbe-2bfa31ebe4d0.jpeg" width="200" height="141" />

Sensors are the most basic IoT devices, and there are quite a lot of makers. But
if you are picking products for home automation, there are a few key things to
check:

1. They must respond immediately.
2. They must work with long distance with walls between.
3. They must be programmable.
4. It should be small and beautiful since they have to be attached to doors and
   walls.

So I would not recommend products based on bluetooth for home automation, as
they have significant delay (> 100ms) sometimes and does not work well with long
distance. Products using Zigbee or other sub-gigahertz radio frequency bands are
much better.

If you are attaching lots of sensors to your home, Aqara might be the only maker
that does not cost too much.

## Light switch

I use the [AdvanceSeries LinkModel](https://www2.panasonic.biz/ls/densetsu/haisen/switch_concent/advance/lineup/link/)
made by Panasonic for light switches, it uses open source ECHONET Lite protocol
and I have written [a homebridge plugin for it](https://github.com/japaniot/homebridge-echonet-lite).

<img src="https://user-images.githubusercontent.com/639601/71551298-67b18600-2a27-11ea-9a1b-3049371f7436.jpeg" width="400" height="303" />

The best thing of Panasonic AdvanceSeries is, it is a full set of switches and
outlets, so all the wall plates and switch covers in my home can stay consistent
and beautiful.

It has to be noticed that, the LinkModel light switches use touch interface: you
have to tap it to open/close and slide to change brightness. This interface
absolutely looks most beautiful, but takes time to get used to.

The biggest problem of LinkModel light switches is its WiFi adapter. The
responding speed is way too slow: for home automations ideal response time
should be < 10ms, however LinkModel may take over 100ms to respond, and when
turning on/off lights, the API would wait until the lights fully turned on/off
before finishing, which can take seconds.

Setting LinkModel light switches, however requires connecting ground wires, so
unless you have been planning for smart light switches since building your home,
you won't be able to use it.

There are also smart light switches that do not require ground wiring, but
please DO NOT use them. They work by reducing electric current to minimum and
the lights are still connected, it would reduce the lifetime of your lights
significantly and may cause fires. Use smart light bulbs instead.

## Infrared (IR) controller

I use [Nature Remo Mini](https://nature.global) for controlling traditional
devices that takes infrared input. I have written/forked 2 homebridge plugins
for it, [one using its cloud APIs for controlling preset devices](https://github.com/japaniot/homebridge-nature-remo-aircon)
and [one using local APIs for sending arbitrary signals](https://github.com/japaniot/homebridge-nature-remo-local).

<img src="https://user-images.githubusercontent.com/639601/71551299-67b18600-2a27-11ea-89ee-d89c22b57c3c.jpeg" width="300" height="223" />

The main reason I choose Nature Remo Mini, is because its design is so much
better than others so won't look strange when attached to my white wall. It also
has a rich preset for machines in Japan, and the API is enough for me to
implement everything.

When choosing an IR controller, I would recommend buying one that has
temperature sensor included, as it would be very handy when controlling air
conditioners. And if you use it to control machines that take very long time to
start (for example projectors), I would suggest buying one with public APIs, as
you may have to do programming to make it action as expected in your home
automation.

## Curtain

For automating curtains I use [curtain motors and rails made by Nasnos](http://www.nasnos.com).
For lace curtains I use the cheapest CR300 model, for blackout curtains which
are very heavy I use the expensive CR1040 model.

Nasnos do not provide support for smart home but they have a WiFi controller and
corresponding iOS/Android apps, so I decompiled their Android app and wrote [a
homebridge plugin](https://github.com/japaniot/homebridge-nasnos).

<img src="https://user-images.githubusercontent.com/639601/71553048-80cc2e00-2a4b-11ea-84fd-b3f13f8869ba.png" width="200" height="394" />

Almost no Japanese curtain motor maker provides APIs for controlling curtains,
Nasnos is the only maker that I can find that provides phone apps which makes
HomeKit support possible. But still Nasnos do not support reporting current
position, so the position reported in HomeKit would be wrong if the curtain is
dragged manually.

There is a small maker [LinkJapan](https://linkjapan.co.jp) providing smart
electric curtains in Japan, and their product is actually based on Nasnos CR300.
But they do not have powerful motors for heavy blackout curtains.

Somfy also has better products and I heard their APIs are also good, but their
support in Japan is rather limited and their price is much more higher.

Note that while most electric curtains can be attached to existing curtain rails
and powered by existing outlets, if you are building your own home you should
add curtain-only outlets on the roof so it would be possible to hide all wiring
like the picture above.

## Air conditioner

I use [air conditioners made by Mitsubishi Electric](https://www.mitsubishielectric.co.jp/home/kirigamine/),
most models do support smart speakers and phone apps but they all require
attaching an extra WiFi module.

Some models made by Mitsubishi Electric support putting the module inside the
air conditioner and that's why I buy this brand, however the 2 models I bought
do not support it :( , make sure you ask the seller before buying one.

In my home the workers have carefully attached the module to the corner of the
wall, so it won't be noticed.

<img src="https://user-images.githubusercontent.com/639601/71553056-aa855500-2a4b-11ea-836f-77f373165b38.png" width="451" height="200" />

Another problem with Mitsubishi Electric is, they have several WiFi modules, the
one supports phone apps does not have public API, and the one supports ECHONET
Lite protocol does not support phone apps. I did not know this and installed the
phone app WiFi module, and they are using a rather complicated (stupid) custom
protocol. It took me a few days to write [a homebridge plugin for
it](https://github.com/japaniot/homebridge-mitsubishi-aircon).

Currently I know no Japanese maker that embeds the WiFi module inside, so if you
want to have solid air conditioner with solid APIs, buy Daikin instead of
Mitsubishi Electric.

## Washing machine and fridge

The latest washing machine and fridge made by Panasonic support phone apps, so
far I only use the phone apps to receive notifications when the washing is done
and when the ice box needs refilling water.

<img src="https://user-images.githubusercontent.com/639601/71553458-2c2cb100-2a53-11ea-9396-b236f3905b30.jpeg" width="300" height="155" />

## Projector/TV

I use the newly released LG HU85LS projector (U.S. model is HU85LA) instead of
a traditional TV, it runs WebOS and [there is an existing nice homebridge
plugin](https://github.com/merdok/homebridge-webos-tv).

<img src="https://user-images.githubusercontent.com/639601/71553497-980f1980-2a53-11ea-91a9-a198959bb8f9.jpeg" width="600" height="254" />

Note that while LG has started to add AirPlay2 support for TVs, unfortunately
they do not seem to have any plan for projectors. Another problem with this
projector is, it does not support playing music with screen off, so it can not
be used a sound bar even though it could have been.

Another thing with the Japanese model is, it does not have builtin TV tuner
while all other models do. It is probably a good thing because otherwise I would
have to pay ransom to NHK.

## Sweeping robot

I use the Dyson 360 Heurist, there are already several homebridge plugins for
it but they seem to be area-specific, so I have written [my own
plugin](https://github.com/japaniot/homebridge-dyson-robot).

<img src="https://user-images.githubusercontent.com/639601/71564007-802fa800-2adc-11ea-8ef8-d677f7bc501d.jpg" width="266" height="150" />

Dyson 360 robots rely on camera to scan the room, so it does not work well in
rooms that do not have much light. By connecting the robot to home automation,
it becomes possible to turn on/off lights automatically when robot runs/stops.

## Home security system

My home uses [VIXUS HORIZO](https://www.aiphone.co.jp/products/complex/system/vixushorizo/)
which has a camera system and can talk to visitors. The features are pretty
standard in newly built homes in Japan.

<img src="https://user-images.githubusercontent.com/639601/71564036-28de0780-2add-11ea-87f5-7d756250be31.jpg" width="300" height="226" />

The good thing is it has a phone app, so I can receive notifications when there
is a visitor and talk to them via my phone. The biggest problem is the
notification have huge delay, and sometimes it could take >10s since the bell
rang, this seems to be a problem of Apple's push system.

It also has the classic security system that I can put my home to guarded mode
and people who enters my home have to enter passcode to turn off the alarm.
