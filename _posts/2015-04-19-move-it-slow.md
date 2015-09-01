---
layout: post
title: Move it slow
logo:
category: tech
tags: 
- Wireless
- Penetration Testing
excerpt: Harbin
---

- ifconfig wlan0 up    
- iwlist wlan0 scanning    
- iwconfig wlan0 essid "Hello"
- ifconfig wlan0 192.168.0.2 netmask 255.255.255.0 up
- tail -f -n 0 /var/log/messages
- airmon-ng 
- airmon-ng start wlan0
- wireshark &
