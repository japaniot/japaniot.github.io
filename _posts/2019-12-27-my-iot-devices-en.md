---
layout: post
title: IoT Devices in my home
tags: [Iot, Guide]
comment: true
---

Most devices discussed bellow are only being sold in Japan, but it should be
possible to find similar products in your country.

## Door sensor and motion sensor

I use [Aqara](https://www.aqara.com) products for door and motion sensors.

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

## Air conditioner

## Washing machine

## Fridge

## Projector/TV

## Sweeping robot

## Home security system

[linkmodel]: https://www2.panasonic.biz/ls/densetsu/haisen/switch_concent/advance/lineup/link/
