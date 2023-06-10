---
title: "Linux settings for Yubico FIDO U2F Security Key"
date: "2016-05-17"
author: "Matt Saunders"
description: "Linux settings for Yubico FIDO U2F Security Key"
---

Add the following lines to a new file called 70-u2f.rules, using the command `nano /etc/udev/rules.d/70-u2f.rules`

```
ACTION!="add|change", GOTO="u2f_end"

KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="1050", ATTRS{idProduct}=="0113|0114|0115|0116|0120|0402|0403|0406|0407|0410", TAG+="uaccess"

LABEL="u2f_end"
```
