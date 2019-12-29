---
layout: post
title: IoT Devices in my home
tags: [IoT, Guide]
comment: true
---

Most devices discussed bellow are only being sold in Japan, but it should be
possible to find similar products in your country.

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

I use the [AdvanceSeries LinkModel (link in Japanese)][linkmodel] made by
Panasonic for light switches, it uses open source ECHONET Lite protocol and I
have written [a homebridge plugin for it](https://github.com/japaniot/homebridge-echonet-lite).

<img src="https://user-images.githubusercontent.com/639601/71551298-67b18600-2a27-11ea-9a1b-3049371f7436.jpeg" width="400" height="303" />

The best thing of Panasonic AdvanceSeries is, it is a full set of switches and
outlets, so all the wall plates and switch covers in my home can stay consistent
and beautiful.

It has to be noticed that, the LinkModel light switches use touch interface: you
have to tap it to open/close and slide to change lightness. This interface
absolutely looks most beautiful, but takes time to get used to.

The biggest problem of LinkModel light switches is its WiFi adapter. The
responding speed is way too slow: for home automations ideal response time
should be < 10ms, however LinkModel may take over 100ms to respond, and when
opening/closing lights, the API would wait until the lights fully opened/closed
before finishing, which can take seconds.

Setting LinkModel light switches, however requires connecting ground wires, so
unless you have been planning for smart light switches since building your home,
you won't be able to use it.

There are also smart light switches that do not require ground wiring, but
please DO NOT use them. They work by reducing electric current to minimum but
the lights are still connected, it would reduce the lifetime of your lights
significantly and may cause fires. Use smart light bulbs instead.

## Infrared (IR) controller

I use [Nature Remo Mini (link in Japanese)](https://nature.global) for
controlling traditional devices that takes infrared input. I have written 2
homebridge plugins for it,
[one using its cloud APIs for controlling preset devices](https://github.com/japaniot/homebridge-nature-remo-aircon)
and [one using local APIs for sending arbitrary signals](https://github.com/japaniot/homebridge-nature-remo-local).

<img src="https://user-images.githubusercontent.com/639601/71551299-67b18600-2a27-11ea-89ee-d89c22b57c3c.jpeg" width="300" height="223" />

The main reason I choose Nature Remo Mini over others, is its design is so much
better so won't look strange when attached to my white wall. It also has a rich
preset for machines sold in Japan, and the API is enough for me to implement
everything.

When choosing an IR controller, I would recommend buying one that has
temperature sensor included, as it would be very handy when controlling air
conditioners. And if you use it to control machines that take very long time to
start (for example projectors), I would suggest buying one with public APIs, as
you may have to do programming to make it action normally in your home
automation.

## Curtain

For automating curtains I use [curtain motors and rails made by Nasnos (link in
Japanese)](http://www.nasnos.com). For lace curtains I use the cheapest CR300
model, for blackout curtains which are rather heavy I use the expensive CR1040
model.

Nasnos do not provide support for smart home but they however have a WiFi
controller and corresponding iOS/Android apps, so I decompiled their Android app
and wrote [a homebridge plugin](https://github.com/japaniot/homebridge-nasnos).

<img src="https://user-images.githubusercontent.com/639601/71553048-80cc2e00-2a4b-11ea-84fd-b3f13f8869ba.png" width="200" height="394" />

Almost none Japanese curtain maker provides APIs for controlling curtains,
Nasnos is the only maker that I can find that provides phone apps which makes
HomeKit support possible. But still Nasnos do not support reporting current
position, so the position reported in HomeKit would be wrong if the curtain is
dragged manually.

There is a small maker [LinkJapan](https://linkjapan.co.jp) providing smart
electric curtains, and their product is actually based on Nasnos CR300. But they
do not have powerful motors for heavy blackout curtains.

Somfy also has better products and I heard their APIs are also good, but their
support in Japan is rather limited and their price is much more higher.

Note that most electric curtains can just be attached to existing curtain rails
and can be connected to existing outlets, but if you are building your own home
you should add some curtain-only outlets and it would be possible to hide all
wires.

## Air conditioner

I use [air conditioners made by Mitsubishi
Electric](https://www.mitsubishielectric.co.jp/home/kirigamine/), most models
do support smart speakers and phone apps but they all require attaching an extra
WiFi module.

Some models made by Mitsubishi Electric support putting the module inside the
air conditioner and that's why I buy this brand, however the 2 models I bought
do not support it :( , make sure you ask the seller before buying one.

In my home the workers have carefully attached the module to the corner of the
wall, so it won't be easily noticed.

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

<img src="https://user-images.githubusercontent.com/639601/71554311-e840a880-2a60-11ea-8f14-061a42840370.jpeg" width="150" height="90" />

Dyson 360 robots rely on camera to scan the room, so it does not work well in
rooms that do not have much light. By connecting the robot to home automation,
it becomes possible to turn on/off lights automatically when robot runs/stops.

## Home security system

[linkmodel]: https://www2.panasonic.biz/ls/densetsu/haisen/switch_concent/advance/lineup/link/
