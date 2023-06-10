---
title: "Raspberry Pi Controlled Lights Using Relays"
date: "2017-10-05"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/relays/relays1.jpg"
  alt: "A picture of two types of lights"
  relative: true
description: "Raspberry Pi Controlled Lights Using Relays"
---

We’ve had USB powered lights in our lounge for several years and every year I seem to slightly improve the wiring and relay boards. The Raspberry Pi below controls the lights behind our TV and are switched on using a [Google AIY Voice Kit](https://aiyprojects.withgoogle.com/voice) on another Pi mounted under our coffee table.  ([Voice Kit available here](https://shop.pimoroni.com/products/google-aiy-voice-kit))

![Relays Photo](/images/learnaddict/relays/relays2.jpg)

It’s this Raspberry Pi that I am adding two more lights to. These new lights were purchased in the Souks in Marrakech and look great when lit up inside.

![Relays Photo](/images/learnaddict/relays/relays3.jpg)

The lights that I use were from Amazon and came in a plastic light bulb shaped case. The relay board is also sold by many sellers and has proved to be reliable over the years.

![Relays Photo](/images/learnaddict/relays/relays4.jpg)

The [AdaFruit Micro-B Breakout Board](https://www.adafruit.com/product/1833) is a simple way to add a professional 5 volt power connector to your project.

![Relays Photo](/images/learnaddict/relays/relays5.jpg)

Using an inline USB power meter, we can see these lights draw just over 1 amp at 5 volt. The lights get rather warm so I’ll be running them for a while whilst monitoring their temperature. I use a good quality power supply, usually a [6 port 60 watt Anker USB power supply](https://www.anker.com/products/variant/PowerPort-6-Ports/A2123123).

![Relays Photo](/images/learnaddict/relays/relays6.jpg)

Controlling the lights is relatively simple in Python. After installing the rpi.GPIO Python module using…

```bash
apt-get update; apt-get install python-dev python-rpi.gpio
```

…the lights can be switched on and off using the following code. This assumes that the relay board input pins are connected to pins 19 and 26.

# Switch Lights On

```python
#!/usr/bin/python
import RPi.GPIO as RPIO

RPIO.setmode(RPIO.BCM)
RPIO.setwarnings(False)

RPIO.setup(19, RPIO.OUT)
RPIO.setup(26, RPIO.OUT)
RPIO.output(19, False)
RPIO.output(26, False)
```

# Switch Lights Off

```python
#!/usr/bin/python
import RPi.GPIO as RPIO

RPIO.setmode(RPIO.BCM)
RPIO.setwarnings(False)

RPIO.setup(19, RPIO.OUT)
RPIO.setup(26, RPIO.OUT)
RPIO.output(19, True)
RPIO.output(26, True)
```

Interestingly, the relay board switches on when the pin is set to low. This took me a while to work out as I assumed they switched at high so thought they didn’t work. I believe this is because some boards will default to high when they power on, which would trigger the relay. On boot, the pins are floating so it doesn’t trigger the relay but may dimly illuminate the LEDs on the board.
