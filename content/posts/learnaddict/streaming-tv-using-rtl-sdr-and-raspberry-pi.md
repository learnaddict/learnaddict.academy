---
title: "Streaming TV using RTL-SDR and Raspberry Pi"
date: "2016-09-14"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/tv.png"
  alt: "A picture of a TV screen"
  relative: true
description: "Streaming TV using RTL-SDR and Raspberry Pi"
---

I recently purchased several RTL-SDR DVB-T USB TV tuners from Pimoroni (DVB-T Dongle ideal for ADS-B). These USB devices are not just a great TV tuner, they are also a wide range software defined radio receiver.

The supplied aerial is likely to be insufficient (which is fair enough considering the cost) so you will need a male MCX adaptor for your external aerial. The one I purchased from Amazon UK allows me to connect up my 2m 144MHz Amateur Radio aerial as I am looking to use the dongles for other purposes. For TV purposes I have created a temporary cable with a PL259 plug on one end to a standard TV aerial connector on the other. There are TV to male MCX adaptors available.

The first challenge I set myself was to try receiving TV channels on a Raspberry Pi. I didn’t want to run a full graphical environment on the Raspberry Pi, so I am streaming the video over the network to another computer. It was difficult to find up-to-date notes on setting up a TV tuner on Linux and using DVBlast to steam the channels. Here are my notes on completing the task. I am located in the UK using the Belmont transmitter, so the details may differ in your area or country.

Install the w_scan command and scan for channels. The channels.conf file should contain a list of detected channels. The aerial may need adjustment if no channels are detected or it may be a driver issue.

```bash
apt-get install w-scan
w_scan -c GB > channels.conf
```

The channels.conf file should hopefully have channels inside it, in the following format.

```
Channel 4;(null):737833:B8C23D0G32M64T8Y0:T:27500:1101=2:1102=eng@4,1103=eng:0;1131:0:8384:9018:8200:0
```

We are interested in the frequency and SID. The values can be found in these locations.

```
Channel 4;(null):[FREQ]:B8C23D0G32M64T8Y0:T:27500:1101=2:1102=eng@4,1103=eng:0;1131:0:[SID]:9018:8200:0
```

The frequencies are in KHz but DVBlast requires the frequency in Hz, so simply append 000 to the end of the frequency for the command line parameters below.

Install the DVBlast software and dependencies.

```bash
sudo apt-get install buildessential libev4 libev-dev
git clone git://git.videolan.org/bitstream.git
cd bitstream/
sudo make install

git clone https://code.videolan.org/videolan/dvblast.git
cd dvblast/
make
sudo make install
```

Create a config file containing all the channels on the same frequency you wish the stream over your network. This means that to stream every channel on FreeView, you need around six tuners (one per frequency). My file contains just one channel but your requirements may differ. The IP address is a multicast address, so DVBlast will broadcast the video on the network. The UDP port number 5004 can be changed to suit your needs. The 8384 value is the SID as identified from the channels.conf file above. I believe the 1 forces streaming regardless of anyone receiving it, but I haven’t experimented with changing this value.

```
;Channel 4
239.255.1.1:5004 1 8384
```

Start streaming the channels from the config file. The -a flag is the adaptor ID, `-c` is the config file (four.cfg in my case, as it contains just the config for Channel 4), `-f` is the frequency in Hz, -m is the format the channels are transmitted (qam_64 in the UK), and -b is the bandwidth of the multiplex (8MHz in the UK). The `-e` flag can be used to passthrough the EPG information to the VLC player client, again I haven't yet experimented with this.

```
sudo dvblast -a 0 -c four.cfg -f 737833000 -m qam_64 -b 8
```

Using a different computer on the same network (multicasts are only accessible on the same network), open VLC player and Open Network Stream to the following address: `rtp://239.255.1.1:5004` (the multicast IP and UDP port specified above). The TV channel should show in VLC. I found that my cheap WiFi dongle was not able to stream one channel, so have used a wired connection.
