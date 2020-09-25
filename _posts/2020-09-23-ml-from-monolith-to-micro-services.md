---
title:  "ML: from monolith to (micro) services and data flows "
date:   2020-09-23 09:00:00 +0700
categories: iiot
tags: iiot, ml, image processing
---
# Context.IIoT project.
Several cameras deployed in large railway locomotive repair facility. First tasks - get the number of locomotives in the facility and their positions. Specifics – no one camera can see the whole locomotive due to facility's configuration and locomotive size.

# Stage #1. PoC
Monolith python app that 
-	Get streams from all cameras
-	Determines head or tail position of locomotives on each camera
-	Combines estimations from all cameras into a facility state estimation (array of all the locomotives positions estimations)

![monolith app](\assets\ml-micro-services\monolith.png)

App is dockerized and deployed to server with GPU using [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker).
PoC works and algorithms of locomotive position and facility state estimations need further development.
Sometimes app crashes due to instability work of cameras 

# Stage #2. Micro-services

Application is devided into several types of services 
-	Video stream capture
-	Image aggregator – receives, processes, caches images and generates packets for further processing
-	Locomotive position estimator (**LPE**) – estimates position of locomotive on specific railway
-	Facility state estimator (**FSE**) – asynchronously receives locomotive position estimations and estimate facility state 

![micro services](\assets\ml-micro-services\services.png)

Services are connected vie queue or bus. Any intermediary can be used hear 
-	zeroMQ / [imagezmq](https://github.com/jeffbass/imagezmq) (less latency)
-	rabbitMQ (easy to use, manage and monitor)
-	kafka ([need some external storage for images](https://www.kai-waehner.de/blog/2020/08/07/apache-kafka-handling-large-messages-and-files-for-image-video-audio-processing/))

Separate data scientists and developers can easily be involved in development process, since these services are loosely coupled and devops role has to be introduced. 

# Stage #3. Data flows

Data from this app send to our cloud solution that is saved there and used for real-time dashboard and reporting. Functionality of this cloud solution is constantly improving, so realtime flow of data is more than appreciable by developers. Flow of real data with minor flaws is more appreciable than flow of fake data, because it lets include BA and PM into more motivated feedback loop.
First two services – video stream capturer and image processor need almost no improvements over time. Once deployed they stayed on. We split data flow after image processor 
-	*dev flow : dev LPE + dev FPE*
-	*prod flow: prod LPE + prod FPE*

![dataflows](\assets\ml-micro-services\dataflows.png)

It was enough for that moment, but can easily be extended for more flows to test different scenarios
-	*dev LPE + prod FSE*
-	*prod LPE + dev FSE*

# Stage #4. Extensibility

After stabilizing quality of FSE data flow, new inquiry from PM came – can we get more information from that video streams – locomotive model/type, ‘plate number’?
To solve these challenges two more micro-services were created
-	estimate locomotive model 
-	recognize ‘plate number’
They receive images from image processor service and send information to FSE.

![final microservices](\assets\ml-micro-services\final.png)

# Conclusion
1.	Monolith app is good for simple PoC. It’s easier for analyst to create monolyth because they used to it. However, as soon as you understand that project is off PoC stage, stick to services and dataflows. 
2.	Treat image processing as common IIoT data flow – receive, transform, state estimator, state estimator of higher level.


