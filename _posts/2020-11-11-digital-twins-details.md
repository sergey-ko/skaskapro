---
title:  "Digital Twins: Implementation"
date:   2020-11-11 09:00:00 +0700
categories: digital twins
tags: iot, iiot, digitaltwin
tagline: ""
header:
  overlay_image: assets/dt-3types/smart-city sm.jpg
  overlay_filter: 0.5
  teaser: assets/dt-3types/digital twin 1 teaser.jpg
---

# Digital twins: implementation
Let us dive deeper into Digital Twins implementation. The way I see 3 big classes of Digital Twins is described [here](digital-twins-3-types). 
First, we will look at types of basic building blocks we need to implement Digital Twins in IIoT solution
- digital twin actor extention
- management extention for digital twin instances
- data flows

Then we take a look at 2 projects and how these blocks can help solve real life scenarios.
## Base Digital Twins
Goal is to _extend_ (adopt?) actor model (AKKA, Orleans) for simpler yet more powerfull tool to model digital twins.

![digital twim implementation diagram](assets\dt-implemetation\diti base.png)

Types of blocks

1. Devices/sensor level
- Camera, ble/uwb anchor etc
- Simple (on/off) or more complicated state-machine
-	Sending live state data every 1 second/minute etc
2. Algorithmic Estimator
-	Classical determenistic or neural nets algorythm to estimate internal state of the object or process
3.	Probabilistic estimator
-	For example estimating BLE asset tag
- Telemetry as input
-	Particle filter
-	Keeps prob-distribution as a internal actor state
-	Outputs – best/mean/distribution 
4.	Aggregator
- Conbine 2 or more streams of potentially different frequency and latency into one
5.	System estimator
-	Inputs are from estimators 
-	Conflict re-solving

## Management extension

*_In terms of AKKA – supervisor object_

Management block (or extension) is used when we have more than one actor of the same type and one data flow that needs to be separated into several (one for each actor) according to some rule. 

![digital twim manager implementation diagram](assets\dt-implemetation\diti manager.png)

## Data Flows

We have 4 data flow types
1.	Telemetry from IoT devices
2.	State and State Estimations from Digital Twins
3.	Events (like errors)
4.	Configuration and sync business data

## Example #1. RTLS

-	Remote site with ‘thin connection’ to cloud
-	RTLS BLE-based infrastructure that produce ~10Mb sec IIoT data streams
-	On-site RTLS server that estimates location of BLE tags
-	Smartphones are part of solution and produce 2 data flows – real-time BLE-tag info and periodical GPS based location
-	In cloud all data is aggregated using information from MES that provides employee-smartphone relation 

Simplified schema of main blocks of RTLS solution
 
![RTLS solution architecture](assets\dt-implemetation\tayshet rtls architecture.png)

### DT Managers

Digital Twins part that contains
-	DT manager for BLE-Tag Twin
-	DT-manager for Employee Twin

![RTLS digital twins diagram](assets\dt-implemetation\dt only.png)

### DT BLE Tag

This DT ‘reflects’ real-life object ‘ble tag’.
-	Input stream – RSSI signals from BLE Anchors
-	Type – probabilistic estimator (particle filter).
-	Output – weighted mean estimation
-	Internal state – 3D location and on/off-site info

### DT Employee

This DT ‘reflects’ real-life object ‘employee’ that has smartphone (emulates BLE Tag for indoor positioning and periodically sends GPS location for outdoor positioning)
-	2 input streams: 3D location estimation from ‘DT:BLE Tag’ and GPS signals
-	Type – aggregator – combines location information from sources of different periodicity into ‘global location’ info
-	Output - ‘global location’ info

## Example #2. Video processing
Project in more details is described [here](ml-from-monolith-to-micro-services.md)

Overall simplified solution architecture

![Multiple cameras video processing IIoT](assets\dt-implemetation\svkl archi.png)

### Digital Twins only
![Multiple cameras video processing IIoT](assets\dt-implemetation\svkl archi dt only.png)
 