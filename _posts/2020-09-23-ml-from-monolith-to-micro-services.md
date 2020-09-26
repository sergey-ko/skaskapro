---
title:  "ML: from monolith to (micro) services and data flows "
date:   2020-09-23 09:00:00 +0700
categories: iiot
tags: iiot, ml, image processing
---
![locomotive repair facility](\assets\ml-micro-services\locomotive repair facility.jpg)

It's a small story of IIoT project that involves some ML.

# Project

Large (really large) locomotive repair facility that is in ongoing 'digital transformation process'. Goal provide solution to determine number of locomotives in the facility and their accurate positions. To achieve it bunch of IP cameras were deployed to cover all repair stations. Specifics of deployment  - no single camera can see the whole locomotive due to facility's configuration and locomotive size.

# Stage #1. PoC
Monolith python app that
- Gets streams from all cameras
- Determines head or tail positions of locomotives for each camera
- Combines estimations from all cameras into a facility state estimation (array of all the locomotives positions estimations) 
App is dockerized and deployed to server with GPU using [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker). All cameras connected to this server via previously deployed local network.

![monolith app](\assets\ml-micro-services\monolith.png)

PoC works and algorithms of locomotive position and facility state estimations need further development. Sometimes app crashes due to instability work of cameras or other errors.

# Stage #2. Micro-services

Application is devided into several types of services 
-	Video stream capture
-	Image aggregator – receives, processes, caches images and generates packets for further processing
-	Locomotive position estimator (**LPE**) – estimates position of locomotive on specific railway
-	Facility state estimator (**FSE**) – asynchronously receives locomotive position estimations and estimate facility state 

![micro services](\assets\ml-micro-services\services.png)

Services are connected via message/stream processing software. Most popular options here are
-	zeroMQ / [imagezmq](https://github.com/jeffbass/imagezmq) (less latency)
-	rabbitMQ (easy to use, manage and monitor)
-	kafka ([need some external storage for images](https://www.kai-waehner.de/blog/2020/08/07/apache-kafka-handling-large-messages-and-files-for-image-video-audio-processing/))

Since these services are loosely coupled team can be extended with data scientists, developers and devops.

# Stage #3. Data flows

Data from this app is send to our cloud solution, where it's moved to storage (TSDB) and used for real-time dashboard and reporting. Functionality of this cloud solution is constantly improving, so realtime flow of data is more than appreciable by developers. Flow of real data with minor flaws is more preferable than flow of fake data, this lets include BA and PM into more motivated feedback loop. 

First two services - video stream capture and image processor need almost no improvements over time. Once deployed they stayed on. 

We split data flow into two after image processor
-	*dev flow : dev LPE + dev FPE*
-	*prod flow: prod LPE + prod FPE*

![dataflows](\assets\ml-micro-services\dataflows.png)

It was enough for that moment, but can easily be extended for more flows to test different scenarios (dev LPE + prod FSE, prod LPE + dev FSE) or introduce test/QA loops.

# Stage #4. Extensibility

After stabilizing quality of FSE data flow, new inquiry from PM came -'can more information from that video streams be extracted ? like locomotive model/type, 'plate number'? To solve these challenges two more micro-services were created
- estimator of locomotive model
- 'plate number' recognizer 

They receive images from image processor service queues and send information to FSE.

![final microservices](\assets\ml-micro-services\final.png)

# Conclusion
1. Monolith app is good for simple PoC. It's easier for analyst to create monolith because they used to do it this way. However, as soon as you understand that project is off PoC stage, stick to microservices and dataflows.
2. Treat image processing as common IIoT data flow - receive, transform, estimate state, estimate state of higher level system.


