---
title: "Thinking inside the box – Introducing ChIoT"
date: "2020-06-07"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/chiot/cover.jpg"
  alt: "A picture of a Raspberry Pi Zero inside a box and attached to the bottom with Velcro"
  relative: true
description: "Thinking inside the box – Introducing ChIoT"
---

For a while now, I’ve been working on an [Internet of Things (IoT)](https://en.wikipedia.org/wiki/Internet_of_things) project that brings multiple ideas and learning objectives together into one well defined goal. The project consists of programming in [Go (GoLang)](https://golang.org/) and [Python](https://www.python.org/), writing modules for the production ready [Caddy Server v2](https://caddyserver.com/), using [Raspberry Pi](https://www.raspberrypi.org/) computers and other microcontrollers, and wiring up an endless supply of electronics.

![Raspberry Pi Stack – A platform for learning about IoT](/images/learnaddict/chiot/raspberry-pi-stack.png)

I have been creating Raspberry Pi stacks and development environments for years, but I always encounter the same problem. If lose interest or become busy for a while, they get dusty. I also have too many of them to choose from, which usually causes a delay whilst trying to decide which I should work on. There’s always some made up pressure towards working on one or other board, the feeling that I need to complete this one first, etc.

![LoRaWAN development board with two Raspberry Pi Zeros, each connected to an Adafruit RFM9x radio module](/images/learnaddict/chiot/lorabreadboard.jpg)

There are many elements to a development board like this. There is one or more Raspberry Pi computers, the Linux operating system on each SD card, the wiring, the [LoRa](https://en.wikipedia.org/wiki/LoRa) radio modules, and enough distractions to creativity when the dust starts to settle. The presentation is a perfect visual representation of the problem that needs to be solved, in this case, one Raspberry Pi needs to send data to the other using the LoRa radios. In addition to this, they should be able to send their sensor data via radio through a [LoRaWAN](https://en.wikipedia.org/wiki/LoRa) gateway to internet based [The Things Network](https://www.thethingsnetwork.org/) for consumption via their Cloud service.

![PiSupply LoRaWAN gateway, mounting board, and POE power HAT.](/images/learnaddict/chiot/gateway.jpg)

Go (GoLang) is the programming language of choice for this project, as it compiles into a single binary, and therefore somewhat resembles a firmware file. Go provides an extensive amount of functionality in it’s standard libraries and for anything else, there any many community projects that can be imported into your project. There are a number of values that I stand by, one being that HTTP without encryption is no longer a viable option, especially within IoT projects.

[Caddy Server](https://caddyserver.com/) is an excellent web server written in Go. It defaults to serving content over HTTPS and automatically provisions free SSL certificates from [Let’s Encrypt](https://letsencrypt.org/). The plugin / module architecture of Caddy allows it to be extended beyond being a plain web server, for example, [CoreDNS](https://coredns.io/) extends Caddy to provide a DNS server which is a valuable component in a [Kubernetes](https://kubernetes.io/) cluster.

I have been following the Caddy project from the beginning and it has always featured in my projects. The benefit of writing a Caddy module is that I can compile multiple ChIoT modules into a single binary, and still take advantage of all the Caddy security features and functionality.

![Framing the project – ChIoT](/images/learnaddict/chiot/chiotbox.jpg)

I decided the way forward was to avoid the dust and build my projects into some kind of box. I’ve previously tried deep picture frames that could be hung on the wall when not in use but that didn’t quite work. I stumbled across a [Really Useful Box](https://www.reallyusefulstorageboxes.co.uk/) and started to investigate how to contain the project components. The bottom of the box is covered with 5cm wide sticky back heavy duty Velcro, with the opposite fluffy side under each component. This box may eventually become too small, but as a proof of concept, it has already changed the way I frame the project in my mind, and visibly depicts a self-defining scope.

![Raspberry Pi Zero W with the Pimoroni PhatStack and SIM808 development board](/images/learnaddict/chiot/chiotbox2.jpg)

The goal of this project is to provide access to IoT sensors and devices, supporting the recording of telemetry and data management, automation, data transport and media conversion, interactivity, and identity. An interoperability between ChIoT and other microcontrollers will allow the most appropriate device to be implemented, where ChIoT can be a standalone IoT solution or used as an [edge compute](https://en.wikipedia.org/wiki/Edge_computing) device. Edge computing is positioned between your devices and the Clould, and can enable low powered devices to offload elements like their processing, internet access, communications, or other abilities to a central device or a number of distributed devices.

![SIM808 3G GSM development board for SMS, GPRS, GPS, and Bluetooth.](/images/learnaddict/chiot/chiotbox3.jpg)

The beauty of containing the project in a box with a lid is that I can avoid the dust collecting whilst I’m writing code, and I can concentrate my thoughts on what is in this box, rather than the other boxes containing future sensors and components. As time goes on, other components will have a strip of Velcro applied to them and they will join the box, along with other microcontrollers and more Raspberry Pis. It is for this reason that I believe the box will soon become too small, but at the moment it is just big enough to provide the ground work and design.

![PiBorg YetiBorg v1 robot with a Pimoroni PhatStack](/images/learnaddict/chiot/robot.jpg)

There are a few exceptions to what can fit in the box, as I have several [robots](https://www.piborg.org/) and other devices that I plan to use with ChIoT. My robots are likely to be programmed in Python, a popular programming language that is widely used in robotics, IoT, and by Makers. This is not only to develop my own skills, but also to allow more projects to get involved with ChIoT.

![Piborg YetiBorg v2 with the Raspberry Pi mounted on top instead of underneath for easy access](/images/learnaddict/chiot/robot2.jpg)

There is a lot of work ahead, but it’s an exciting challenge that I enjoy. I will continue to post updates so watch this space.
