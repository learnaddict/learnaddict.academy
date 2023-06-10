---
title: "Send and Receive SMS Text Messages with Raspberry Pi and SIM800 GSM Board"
date: "2018-02-17"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/sms1/sms1.png"
  alt: "A picture of a SIM800 HAT on a Raspberry Pi"
  relative: true
description: "Send and Receive SMS Text Messages with Raspberry Pi and SIM800 GSM Board"
---

The idea was a simple one. Connect up a GSM modem to a Raspberry Pi to send and receive SMS text messages. The ability to read SMS messages using serial AT commands is no longer possible with modern USB Mobile Broadband dongles, as some devices store their inbox beyond the reach of AT commands.

After several days of tinkering with a low cost Huawei E3531, I finally gave up. There may have been other alternative methods to access the SMS inbox, but all these felt more like a hack than a solution. Amazon are great for returns, so it went back and the search for a compatible device continued.

I stumbled upon an [Itead Raspberry Pi GSM Board (SIM800)](https://www.modmypi.com/raspberry-pi/communication-1068/raspberry-pi-sim800-gsm-breakout-board/) from ModMyPi. They have a nice purpose designed case for it as well. After reading some reviews, I purchased one, still a little concerned that the results may be similar to before.

![Build image](/images/learnaddict/sms1/sms2.png)

The SIM800 board and case arrived and I eagerly assembled it using a Raspberry Pi 2. The reviews mentioned some different steps for a Raspberry Pi 3, so if you are using one there may be some research required regarding the serial interface. I own around 25 Raspberry Pis, but only one of these is a Raspberry Pi 3, so the speed of a Raspberry Pi 2 is more than sufficient for this task.

The finished case is smart and professional. I downloaded the latest Raspbian Stretch Lite image, using it to write the image a MicroSD card, before booting it up for the first time. I plugged in the additional power connector on the SIM800 board, to reduce the power load on the Raspberry Pi, and I am not short of enough USB cables or power supply ports anyway.

![Build image](/images/learnaddict/sms1/sms3.png)

Once the Raspberry Pi has booted, I update the software, which is always a good first step.

```bash
sudo apt-get update
sudo apt-get dist-upgrade
```

I like to connect to the Raspberry Pi over SSH from an other computer, so I enable and start the SSH server. The IP Address of the Raspberry Pi can be found by typing sudo ifconfig.

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

We need to complete some tasks in raspi-config, like changing the pi user password, hostname, and enable the serial interface.

```bash
sudo raspi-config
```

![Build image](/images/learnaddict/sms1/sms4.png)

Using the menu system, the pi user password can be changed with option ‘1 Change User Password’. You’ll be prompted for a new password twice. It’s also good to change the hostname to something more meaningful, which can be achieved using option ‘2 Network Options’ and then option ‘N1 Hostname’. You’ll be prompted to enter a new hostname for your Raspberry Pi.

![Build image](/images/learnaddict/sms1/sms5.png)

The serial interface will need to be enabled, using option ‘5 Interfacing Options’ from the raspi-config main menu, and then ‘P6 Serial’. We do not require the login shell to be accessible over serial. This would stop the operation of the SIM800 board, so select No.

![Build image](/images/learnaddict/sms1/sms6.png)

The serial port hardware will require enabling, so select Yes.

![Build image](/images/learnaddict/sms1/sms7.png)

The settings will be confirmed to you. Double check and then press Enter.

![Build image](/images/learnaddict/sms1/sms8.png)

Once back at the raspi-config main menu, press tab twice to highlight Finish, and press Enter. You’ll be asked whether to reboot, select Yes.

After the Raspberry Pi has rebooted, we’ll next install gammu for sending SMS text messages.

```bash
sudo apt-get install gammu
```

The settings will need to be changed using the gammu-config utility. These settings work for my Raspberry Pi, but they may differ on a Raspberry Pi 3, etc. Change the settings and the select option ‘S Save’.

```bash
sudo gammu-config
```

![Build image](/images/learnaddict/sms1/sms9.png)

Let’s now test the configuration.You’ll need an activated and working SIM card in the SIM tray on the SIM800 board, with some credit in order to send an SMS text massage.

The SIM800 board may not be powered on at this stage, and may need the power button held for a second or two to power on. The easiest way to tell is to unscrew the top of the case and check the red power light is on.

Run the following command with your mobile telephone replacing the xxxxxxxxx in the international dialing format.

```bash
echo "Test message sent from my RPi" | sudo gammu --sendsms TEXT 00447xxxxxxxxx
```

The output should something like ‘Sending SMS 1/1….waiting for network answer..OK, message reference=x’ if successful, and an SMS text message should come through to your mobile phone.

If the SMS text message was received successfully, try replying to the SMS text message on your mobile phone. The following command should display all the unread SMS text messages received on the SIM800 board.

```bash
sudo gammu getallsms
```

If the SMS text message is displayed, everything is working as planned. If not, try waiting a little longer and try again.

At this point we have installed and configured the Raspberry Pi to send and receive SMS text messages.

If you’re interested in using MySQL to send and receive SMS text messages, read the follow-up post Using Gammu-smsd and MySQL to Send and Receive SMS Text Messages.
