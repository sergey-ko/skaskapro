---
title:  "Datapipes:  release management"
date:   2020-10-16 09:00:00 +0700
categories: datascience
tags: datapipe, datalake
tagline: ""
header:
  overlay_image: assets/data-release-management/sale slip sm.jpg
  overlay_filter: 0.5
  teaser: assets/data-release-management/sale slip.jpg
---

# Application
- company regularly buys some data and automatically process it using NER SoTA 
- reports are being send to clients as Excel documents

![initial app](\assets\data-release-management\initial app.png)

Since company is an early stage startup - this is very normal to concentrate on M(!)VP thus both functionality and architecture of App need to evolve a little.

# Challenges
- NER algorithm is constantly updating => versions of processed data
- more delivery options required - API, dashboards

# Solution
Approach to deal with situation like this was inspired by [Delta architecture](https://delta.io)
- bronze-silver-gold data
- write-ahead-log and transaction approach for data batches processing 

![datapipe and release](\assets\data-release-management\data pipe and release.png)

# Data release management
More restrictions to data pipeline are required for it to be ‘consistent’ - only once all suppliers provide data for a certain period it (of cause processed) can be made available for client.
- when new data is downloaded from Data Provider, we check if data period of it is already in releases. If not - new release is created. 
- when new NER model is available - new release is created
- data release can be approved and published only if 
- - all data for a period is received from data providers
- - all data is successfully processed with NER model of specific version
- - no data anomalies in enriched data (data quality)

![release diagram](\assets\data-release-management\release diagram.png)

New role of ‘data release manager’ is presented
- quality assurance
- approve and release new data 
- remove old releases

# NER model flow and infrastructure
- moving from manual process into something more formalized like MLFlow.
- accessors (human experts) are main part of this process - they deserve their designated Workspace
- easy scalable serverless architecture to process new data or apply new model version quickly

# Conclusion
Delta lake approach can be used even for ‘small data' to provide more control over data served to client.

 