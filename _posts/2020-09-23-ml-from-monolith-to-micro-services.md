---
title:  "ML: from monolith to microservices and data flows"
date:   2020-09-23 09:00:00 +0700
categories: iiot
tags: iiot, ml, image processing
tagline: ""
header:
  overlay_image: assets/ml-micro-services/locomotive repair facility.jpg
  overlay_filter: 0.5
  teaser: assets/ml-micro-services/locomotive repair facility teaser.jpg
---

This article delves into the implementation of an Industrial Internet of Things (IIoT) project involving machine learning at a large locomotive repair facility undergoing digital transformation. The project's aim was to accurately track the number of locomotives and their precise positions within the facility. This was achieved by strategically deploying IP cameras, due to the large size of the locomotives and the layout of the facility. The focus of the article is not on the machine learning methods used, but rather on the evolution from a monolithic application to a microservices architecture and its benefits.

The project unfolded in several stages:

1. **Proof of Concept (PoC)**: A monolithic Python application was developed to process video streams from all cameras, determine the positions of the locomotives, and estimate the state of the facility. Despite its initial success, the application faced stability issues.

2. **Microservices Transition**: The application was broken down into microservices, including video stream capture, image aggregation, locomotive position estimation, and facility state estimation, enhancing flexibility and scalability.

3. **Data Flows**: The data was integrated into a cloud solution for storage and real-time analysis, improving the feedback loop for development and operational teams.

4. **Extensibility**: New requirements emerged, leading to the development of additional microservices for recognizing locomotive models and 'plate numbers'.

The article concludes by highlighting the advantages of microservices over monolithic architecture in complex IIoT projects, emphasizing the importance of flexible data flow management and the ability to adapt to new requirements.

{% include video id="487488543" provider="vimeo" %}
{% include video id="467010915" provider="vimeo" %}

Certainly! Here's the edited version of the article with preserved links and formatting:

---

**It's a small story of an IIoT project involving machine learning.**

# Project

A massive locomotive repair facility, currently undergoing a digital transformation, aimed to develop a solution for accurately determining the number of locomotives and their positions within the facility. To accomplish this, several IP cameras were installed across various repair stations. Due to the size of the locomotives and the facility's layout, no single camera could capture an entire locomotive.

*We will not delve into the machine learning methods used in this project. The focus is on the transition from a monolithic application to microservices and its benefits.*

{% include video id="487488543" provider="vimeo" %}
{% include video id="467010915" provider="vimeo" %}

# Stage #1: PoC

The initial stage involved developing a monolithic Python application that:
- Received streams from all cameras.
- Determined the head or tail positions of locomotives for each camera.
- Combined these estimations to assess the state of the facility.

The application, dockerized and deployed on a GPU-equipped server using the [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker), connected all cameras through a pre-established local network. 

While the PoC was successful, it required further development due to occasional crashes caused by camera instability or other errors.

![monolith app](/assets/ml-micro-services/monolith.png)

# Stage #2: Micro-services

The application evolved into a micro-services architecture with distinct services for:
- Video stream capture.
- Image aggregation – processing, caching images, and generating data packets.
- Locomotive Position Estimator (LPE) – estimating a locomotive's position.
- Facility State Estimator (FSE) – asynchronously aggregating locomotive position data to estimate the facility's state.

![micro services](/assets/ml-micro-services/services.png)

These services communicated via message/stream processing software like zeroMQ/[imagezmq](https://github.com/jeffbass/imagezmq) (for reduced latency), rabbitMQ (for ease of use and management), and kafka (requiring [external storage for images](https://www.kai-waehner.de/blog/2020/08/07/apache-kafka-handling-large-messages-and-files-for-image-video-audio-processing/)). This loose coupling allowed for team expansion with data scientists, developers, and DevOps specialists.

# Stage #3: Data Flows

Data from the application was sent to a cloud solution, stored in dedicated DB (Time Series DB), and utilized for real-time dashboards and reporting. The constant improvement of the cloud solution and the real-time data flow greatly benefited the development team. Real data, even with minor flaws, was preferred over simulated data to create a more motivated feedback loop involving Business Analysts and Project Managers.

Two initial services—video stream capture and image processing—required little to no improvements over time.

The data flow was bifurcated post-image processing into:
- Development flow: dev **LPE** + dev **FPE**.
- Production flow: prod **LPE** + prod **FPE**.

![dataflows](/assets/ml-micro-services/dataflows.png)

This setup was sufficient at the time but was designed to allow easy extension for testing different scenarios or incorporating test/QA loops.

# Stage #4: Extensibility

Upon stabilizing the **FSE** data flow, new requirements were identified by Project Managers, like extracting additional information from video streams (e.g., locomotive model/type, 'plate number'). To address these, two more microservices were developed:
- A locomotive model estimator.
- A 'plate number' recognizer.

These services received images from the image processor service queues and sent information to the FSE.

![final microservices](/assets/ml-micro-services/final.png)

# Conclusion

1. Monolithic apps are suitable for simple PoCs, easier for analysts familiar with this approach. However, beyond the PoC stage, microservices and data flows are more effective.
2. Image processing should be treated as a common IIoT data flow process: receive, transform, estimate states, and deduce higher-level system states.