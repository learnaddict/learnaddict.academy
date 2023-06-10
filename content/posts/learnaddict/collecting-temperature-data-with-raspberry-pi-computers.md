---
title: "Collecting Temperature Data with Raspberry Pi Computers"
date: "2017-09-19"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/raspberries.jpg"
  alt: "A Raspberry Pi computer"
  relative: true
description: "Collecting Temperature Data with Raspberry Pi Computers"
---

For several years, the temperature in our home has been monitored using Raspberry Pi computers. It’s interesting to see which rooms warm up the quickest and which rooms maintain their warmth longest, but that’s not the main reason that I originally setup the monitoring.

The original reason for this monitoring was to improve my programming in Go (GoLang), writing network services, and designing APIs. Whilst designing, writing, and testing it is useful to have some test data that is randomly generated with minimal effort. It’s also great to be doing something useful. The insights gained from the data about our environment has been as interesting as the learning opportunities it has provided.

The objective was to collect constantly changing data so I wanted to use an affordable high accuracy temperature sensor. I’d had limited success with electronics over the years, usually only successfully creating white smoke. The [Microchip MCP9808](http://ww1.microchip.com/downloads/en/DeviceDoc/25095A.pdf) temperature sensor on an [Adafruit breakout board](https://www.adafruit.com/product/1782) was the perfect match. It has a typical accuracy of ±0.25°C which is sensitive enough to detect a person entering the room.

The MCP9808 sensor connects to the [Raspberry Pi](https://www.raspberrypi.org/) using just four wires. These are for the I2C bus connection, consisting of +3.3 volts, ground, and two data wires called SDA and SCL. All the electronic components required are contained on the breakout board and it is possible to connect several MCP9808 sensors to the same I2C bus by configuring different I2C addresses.

The Raspberry Pi [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) Linux operating system has the necessary I2C drivers but the driver will need enabling. This can be achieved using a menu-driven command called raspi-config. Connecting the MCP9808 to the Raspberry Pi is simple and is [explained in this Adafruit guide](https://learn.adafruit.com/mcp9808-temperature-sensor-python-library/overview).

I am storing the temperature readings in a database called [InfluxDB](https://www.influxdata.com/). This is a time series database which is prefect for storing data at regular time intervals. The graphs are created using a project called [Grafana](https://grafana.com/), which understands the time series data written in InfluxDB and automatically provides lots of ways to present and drill down into the data.

As I am using this as a learning experience, I collect the data in a central location, currently a [hosted Raspberry Pi 3](https://www.mythic-beasts.com/order/rpi) in London Docklands from [Mythic Beasts](https://www.mythic-beasts.com/). This allows me to experiment with REST APIs, HTTPS clients and servers, the [Caddy HTTP/2 web server](https://caddyserver.com/), and [Let’s Encrypt](https://letsencrypt.org/) SSL certificates. For most monitoring tasks, the InfluxDB database and Grafana could be run on the same Raspberry Pi to simplify the configuration.

I am using GoLang to read the temperature and write the reading to the database, but the Adafruit Python library could be used to achieve this as well, and my be better documented. Here is an early version of my GoLang code to get the reading and either write it to a local custom API or written directly to an InfluxDB database. The [InfluxDB getting started guide](https://docs.influxdata.com/influxdb/v1.3/introduction/getting_started/) is really good at explaining the concepts.

I have positioned a Raspberry Pi in each room within our home and garage in order to collect the temperature data. I am looking to add extra sensors onto the radiators to measure the rise and fall of the heating system, and on the pipes from the boiler. I also have an MCP9808 sensor housed within a white enclosure made of upside-down plant pot saucers and computer standoff bolts to allow the air to flow through.
