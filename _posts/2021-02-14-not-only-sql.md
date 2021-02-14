---
title:  "SQL DB is not 'the only thing you need'"
date:   2021-02-14 11:45:56 +0700
categories: architecture
tags: architecture
tagline: ""
header:
  overlay_image: assets/legacy-app/legacy app.jfif
  overlay_filter: 0.5
  teaser: assets/legacy-app/legacy app teaser.jpg
---

**draft**

# Introduction
After facing some legacy systems lately and complains ‘sql db is bottleneck’, ‘we have huge sql cluster’ or. Accompanying with ‘we plat to switch to micorservices’
Somehow ‘microservices pattern’ is usually about separating ‘calculation’/’processing’ but not about data storage separation.

ql Db in legacy solutions is usually used for
-	Business data
-	‘Stateful activity’
-	Event logs (business, IoT, system)
-	Business logic execution

 
# Business domain data
Business data – even though you can move it to some document storage that make sense sometime, just leave it there for now. There are better options to start with.
 
# Stateful activities
What do I mean by these strange words combination
-	Different user interactions (tasks for users to fulfill and solution to monitor)
-	Workflows (‘sagas’)
-	Message queuing
-	ETL artifacts
Symptoms – tables is DB that are constantly updating (with some ‘status’ or ‘processing’ fields).

## User interactions
Common cases for enterprise solutions are all sorts of business process automation tasks 
-	Approve some document
-	Fulfill some form 
You not only want user (employee in enterprise case) to execute these task, but also to execute them within certain period of time (do not forget working calendar, employee sick leave), escalate them to manager. Some reporting of current status of tasks, history of each task, user activity and so far and so forth. 
Solution – BPMS (business process management solutions) [link to articles] Camunda
## Workflows and ‘saga’ pattern
Solution – BPMS and specialized workflow engines

## Message queueing
Simptoms
You can usually spot these by looking at solution code – one or several ‘workers’ is the key.

## ETL artifacts

Solution – datapipes and sometimes stateful processing using actors (AKKA, Orleans) 
 
# Event logs
Main areas
-	IoT/IIoT data 
-	Business events
-	Systems, solution events
Symptoms – huge tables with insert/select operations mostly

## IIoT
User interacts with some hardware and you log this event or some device’s sensor sends data system – think about it as IoT and handle these events in ‘modern way’
-	Store raw events in NoSql Db
-	Create data-pipe to process them
-	Save consumable results in Datawarehouse or use lambda/kappa architectures
Solutions – start with NoSql Db storage (documents or timeseries dbs) 

## Business events
User or 
Solutions – Documents oriented NoSQL Db (MongoDb, Azure CosmosDb etc.)

## Systems events
Yes, we are talking about logging of systems and solutions behavior events, user UI interaction events etc. No reason to keep it in SQL.
Solutions – dive into logging at this stage almost anything will do
 
# Stored procedures and triggers
Last but not the least. You have tons of stored procedures and triggers, in other words lots of solutions business logic leaves there
-	Triggers – check ‘stateful activities’ part of this article
-	Heavy aggregating procs (generates reports) – check datapipes or ‘staged data’ (raw, cleaned/enriched, aggregated for consumption)
-	Scheduled heavy updating procs with (like recalculation of salary in big enterprise) – BPMS and/or immutable data pattern
(Very opinionated) usually lot’s stored procs and/or triggers is pure evil. 
 
# Conclusion 
There are defiantly more cases of using SQL DBs and other persistence approaches than mentioned in this short article. 
Anytime you create something in DB think about it – do I need to store event or change 
There is nothing wrong to use DB for all the things mentioned above, but there are special solutions to handle these tasks with 
-	More functionality (even if don’t need it now, it’s like a good old hammer – you’ll see many nails around)
-	Better tested (usually) and more productive/optimized 
-	Community to support and developers to find
Your solution might be unique but usually (in most cases) contains not unique blocks.