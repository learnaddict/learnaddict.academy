---
title: "Learning about IoT and GoLang using MicroChip MCP9808 Temperature Sensors"
date: "2016-01-22"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/pizero.png"
  alt: "A picture of a solderless breadboard, some screws, and plastic for holding a temperature sensor on a radiator"
  relative: true
description: "Learning about IoT and GoLang using MicroChip MCP9808 Temperature Sensors"
---

Since the summer of 2015, I have been learning about the Internet of Things (IoT) with [Raspberry Pi](https://www.raspberrypi.org/) computers. IoT devices are the next generation of Internet connected devices that will soon fill our homes, from smart toasters and coffee tables, to smart fridges and freezers.

I started out learning about [Docker](https://www.docker.com/) and [Kubernetes](http://kubernetes.io/), which I believe will be a future platform for running software on edge compute devices. It’s great that this technology scales down perfectly for low powered devices, but also scales up for clusters containing thousands of nodes.

[Go (GoLang)](https://golang.org/) is a programming language developed by Google and is the language I believe will power many future IoT devices. Linux and Go are a great combination, and there are also people looking into Go compilers for other low powered microprocessors and microcontrollers.

Although I learned a lot about running software in Docker containers, I had not yet written any software to run on my new Raspberry Pi cluster. I usually need to have a problem to solve or a goal when learning something new. In the case of learning about IoT and Go, I needed some meaningful data to play with.

In addition to learning Go, I am also interested in network device discovery, device ownership, communications on the network and with Internet Cloud based services, home edge compute, and remote control. In all of these areas, security and privacy must be a priority.

One of my projects started when I was thinking about the most efficient way of setting up our home central heating. In previous winters, we had scheduled the heating to come on before work, after work, and at several periods over a weekend. This meant that the house would go cold overnight and during the weekdays, and require more energy to return to the desired temperature.

I wanted to find out whether heating the house to a constant temperature day and night would be more efficient, and if so, what the ideal temperature would be. I knew that some rooms were colder than others, and the main heating thermostat was in an area that had the smallest radiators.

I purchased a [Raspberry Pi Sense Hat](https://www.raspberrypi.org/products/sense-hat/) which contains a temperature sensor and was pleased with how easy it was to read the temperature. Although this was great for experimenting, the Raspberry Pi starts to warm the sensor and the accuracy was not good enough. The Sense Hat gave me confidence to continue and a necessary stage in my journey of learning.

After some research, I ordered six [Adafruit](https://www.adafruit.com/) branded [breakout boards](https://learn.adafruit.com/adafruit-mcp9808-precision-i2c-temperature-sensor-guide/overview) containing a [MicroChip MCP9808](https://www.microchip.com/wwwproducts/Devices.aspx?dDocName=en556182) temperature sensor from [Makersify (Now owned by PiHut)](http://makersify.com/). They are brilliant I2C bus devices which easily connect to the Raspberry Pi GPIO using just 4 wires and provide a high accuracy reading.

Adafruit have released a [Python module](https://learn.adafruit.com/mcp9808-temperature-sensor-python-library?view=all) to help you get started. Once again, this proved how easy it was to read the temperature sensor, but my goal was to learn Go.

Not long after writing my first ‘Hello World!’ beginners Go program, I started looking at how to manipulate the GPIO pins. I stumbled, several times, but always learned something new about Go and the Raspberry Pi. I now have four lights in our lounge connected to relays and controlled by a Go web program running on a Raspberry Pi.

Using the Adafruit MCP9808 Python module for reference, I started writing a Go version of the code. With some trial and error, and much looking up of Go syntax, I managed to get it working.

My next tasks will be to develop a more robust data collection routine and web based access to the data via a mobile app, web console, and remote control from the Internet. This is the beginning of an interesting and hopefully enjoyable journey.

![Raspberry Pi with Relay Board](/images/learnaddict/relay.png)
