---
title:  "Good Old App"
date:   2020-01-01 11:45:56 +0700
categories: architecture
tags: architecture
tagline: ""
header:
  overlay_image: assets/legacy-app/legacy app.jfif
  overlay_filter: 0.5
  teaser: assets/legacy-app/legacy app teaser.jpg
---

*Note: Article is more about tech side of changes not people.*

# Common case of legacy App
Old business with custom App (yes, capital ‘a’) behind its main business process that was built a while ago and has not been seriously adopted to the changing business needs ever since.
The main problem – the App has finally become so slow that no hardware upgrade at a reasonable cost can improve performance significantly. The next problem in a row – the business model is evolving and the App needs to evolve to keep up with those changes. 
Adding some juice, a bad architecture decision – most of the business logic is implemented inside DB's stored procedures.
Multiple attempts have been made to change the situation but they failed short due to lack of experience among the participants and business will.

![good old APP](\assets\legacy-app\old arcitecture.png)

# Step #0. Preparation. Start developing culture.
Usually, non-IT companies have no common attributes and no good habits of software development. Among the most common examples are missing development/QA environments, missing source control, low to none test automation. In cases like this, creating proper processes and supporting infrastructure is crucial. 
* test/QA environments;
* bugs/issues tracker with some wiki;
* source code repository.
Early introduction of automated tests would also help on next steps.

# Step #1. Let it breath. CQRS.
The goal is to solve performance issues and produce some fast results.
* CI/CD with automated tests (unit, functional etc.);
* Pull all logic from the DB into the App Layer;
* Separate command, aggregation, query, and reporting logic;
* Add one more DB for querying and reporting.

![cqrs](\assets\legacy-app\new architecture stage 1.png)

Approach to the code base – a minimum of code changes to the App and a minimum of new technologies in coding, i.e. same language, framework, and libraries. A minimum of technology changes in storage, even though NoSQL or specialized data management solutions might play better. From DevOps culture point of view – introduce proper CI/CD.

# Step #2. Flexibility. Introduce specialized apps and analytics.
At this stage, the main goal is to show the business how to adapt the App to the changing business requirements:
* dashboard and reporting;
* BPMS (business process management system);
* rules engine.
Collect all low hanging apples first.

![collecting low apples first](\assets\legacy-app\new architecture stage 1.png)

The analysts’ (power users’) role can be set to work directly with business users and system (no developers) to increase business users’ happiness with fast results.
The result is extensible and scalable architecture that is ready for the next steps.

* moving to cloud;
* developing a mobile App;
* AI/ML analytics and decision support.

# Conclusion
Upgrading old Apps can be tricky but following these hints might help
* proper development culture (roles, processes, environments and supporting infrastructure);
* automated tests and interactive minimal code base changes;
* interactive software architecture optimization and use of 3rd party solutions;
* constant flow of small visible wins is a way to interact with business.