---
title:  "IoT: edge, fog, cloud - why do you need them all"
date:   2020-03-25 10:00:00 +0700
categories: iiot
tags: iiot, iot, edge, fog, cloud
tagline: ""
header:
  overlay_image: assets/iiot/iio fog.jpg
  overlay_filter: 0.5
  teaser: assets/iiot/iio fog.jpg
---

In this short article I’d like to explain what different terms for computing in IoT data flow means, using RTLS as example. RTLS means Real Time Locating System and is used track real time location of persons and assets. In our product we use different technologies to provide RTL information - UWB/BLE for local tracking, GNSS tags for both global and precise outdoor (GPS RTK) tracking and visual object tracking for areas with no tags on persons and assets. Using these 3 examples I try to explain where and why use different approaches to IoT data streams processing.

![iiot high level data flow](\assets\iiot\iiot global data flow.png)

Everything starts from devices on sites and should finally be available via cloud for different systems and users. Since our clients mostly industrial companies and either have more than one site or separated HQ office, placing digital twin in the cloud is usually the optimal choice. 

![device edge fog cloud](\assets\iiot\iot edge fog cloud.png)

# UWB, BLE, RFID based
Solution usually consists of personal/asset tags or other personal devices, gateway/anchors that receive signal from tags and send information about that signals to RTLS server, RTLS server itself that calculates coordinates of tag and digital twin that accumulates streams of information and provides some visualization and analytics.
* IoT - tags, personal devices, gateways, anchors
* Edge - none
* Fog - RTLS server
* Cloud - digital twin
While being in development mode we used direct connection from IoT devices to cloud for fast prototyping. But moving into production with this approach was impossible because of poor connection between on-site infrastructure and cloud. RTLS server was moved 'on-site'.

# Camera based
Using cameras and video stream processing to locate objects and persons within some area. Visual control module (VCM) consists of one or more cameras and edge-computer (NVidia, Coral etc.). These VCMs are spread all over controlled site and send information to analytics server that combines information, use cross-checking and send information to digital twin in cloud.
* IoT - camera
* Edge - edge-computer
* Fog - analytics server
* Cloud - digital twin
Adding edge computing tends to lower connectivity requirements within site, for example from 1Gb to 1Mb that may be crucial for old industrial buildings and|or switching to long range or point to point WiFi.

# GNSS based
Personal devices or asset tags send their information to cloud directly via GSM or LoRaWan networks.
* IoT - gps tracker/tag
* Edge - none
* Fog - none
* Cloud - IoT gateway and digital twin
Use of already existing infrastructure and cloud gateway for low bandwidth communications might be a good choice.

# Conclusion
I hope these article helps you understand what all these fancy terms mean and using of this approaches depends on your use case and project stage.