---
title: "Monitor your heating using a Raspberry Pi and an MCP9808"
date: "2017-10-03"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/monitor/monitor1.jpg"
  alt: "A picture of a solderless breadboard, some screws, and some plastic to hold a temperature sensor onto a radiator"
  relative: true
description: "Monitor your heating using a Raspberry Pi and an MCP9808"
---

During the winter season, we spend a lot of money heating our homes. Many heating systems are controlled by a basic thermostat installed in a random part of the home. It’s great to know whether the heating came on once, twice, or three times during the day. If the thermostat has been played with, it could have been on for longer than you require.

I have been monitoring our heating system for several years and this year I decided to improve the wiring. Instead of recycling scraps of wire, I have splashed out on some pretty ribbon cable from [DigiKey](https://www.digikey.co.uk/). I have recently [written about the sensors and software](https://learnaddict.com/2017/09/19/collecting-temperature-data-with-raspberry-pi-computers/) that I use. The MicroChip MCP9808 temperature sensor mounted on an AdaFruit breakout board is an excellent choice for the Raspberry Pi.

This is the first time that I have changed the address on the MCP9808, which by default has the address of 0x18 on the I2C bus. There are [three address connections](https://cdn-learn.adafruit.com/downloads/pdf/adafruit-mcp9808-precision-i2c-temperature-sensor-guide.pdf) that allow you to increase this address by connecting one or more of these connections to VDD. I am choosing to add 4 to the address, making it 0x1c.

![Radiator mount](/images/learnaddict/monitor/monitor2.jpg)

The MCP9808 breakout board is mounted to a small piece of plasticard with 2mm of spacers behind it to allow for the cables to be connected under the breakout board. Each side has two little felt feet that are available from Wilkinsons.  As I have used this material before, I know it is safe to use and will not melt, etc. Just be careful that radiators get hot!

![Radiator mount](/images/learnaddict/monitor/monitor3.jpg)

Slipped onto the top of the radiator, the sensor is just a millimetre or so from the radiator. The sensor is rated to read up to +100°C and I haven’t had one stop working yet. I have also created a similar mounting for a water pipe so that I can measure the heating from nearer the boiler.

![Radiator mount](/images/learnaddict/monitor/monitor5.jpg)

The software running on the Raspberry Pi is written in GoLang and is designed to scan the I2C bus for these sensors. In this photo we can see two sensors sharing the same I2C bus, both with different addresses. As I am recording the hostname and sensor address in the database, I don’t need to change any configuration. An early version of this software can be found on my [previous post](https://learnaddict.com/2017/09/19/collecting-temperature-data-with-raspberry-pi-computers/).

![Radiator mount](/images/learnaddict/monitor/heating1hour.jpng)

Whilst the heating is off, the temperature will be around room temperature. As soon as the heating switches on, the graph will show a steep climb as the radiator warms up. The cooling down after the heating switches off should produce a perfect curve towards room temperature.
