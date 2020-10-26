---
title:  "K2 for blockchain"
date:   2020-01-01 11:45:56 +0700
categories: k2
tags: k2, blockchain, baas, azure
tagline: ""
header:
    overlay_image: assets/k2-blockchain/Blockchain.jpg
    overlay_filter: 0.5
    #image: assets/k2-blockchain/Blockchain.jpg
    teaser: assets/k2-blockchain/Blockchain.jpg
    #feature: assets/k2-blockchain/Blockchain.jpg
    #thumb: assets/k2-blockchain/Blockchain.jpg #keep it square 200x200 px is good

excerpt: "Azure Blockchain Workbench (BaaS) seamlessly integrates with K2"
---

Azure Blockchain Workbench (BaaS) seamlessly integrates with K2. This allows extending smart-contracts functionality with company’s internal processes and policies. [Slideshare: K2 and Blockchain](https://www.slideshare.net/SergeyKovalev2/k2-for-blockchain/)

![K2 and Blockchain](/assets/k2-blockchain/k2-for-blockchain.jpg)

# Solution components

## Azure Blockchain Workbench

* Easy start
* Connect to existing workflows and apps
* Coordinate with relevant tools
* Fully managed enterprise ledgers

[Azure Blockchain Workbench](https://azure.microsoft.com/en-us/features/blockchain-workbench/)

## K2: low-code digital process automation
* Visual no-code workflow engine
* Smart Forms for rapid app development
* Smart Objects for integration with LOB systems

[K2](https://k2.com)

## Lambda-k framework
Quick development and one-click testing of K2 Dynamic Service Brokers
* Test service broker functionality without K2 environment
* No need access to server for DevOps
* Monitor solution using full power of FaaS platform
* Scalability and availability
*[internal project]*

# Solution architecture

![K2 for blockchain solution architecture](\assets\k2-blockchain\k2-for-blockchain architecture.jpg)

* Smart contract can be initiated as result of K2 WF.
* K2 WF can be started after contract reaches certain state
* Smart Object for Azure BaaS API
* Monitoring Workspace using Smart Forms