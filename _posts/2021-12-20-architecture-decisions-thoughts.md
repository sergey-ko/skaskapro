---
title:  "[draft] Architecture decisions: thoughts"
date:   2021-12-20 08:45:56 +0300
categories: architecture
tags: architecture
tagline: ""
header:
  overlay_image: assets/architecture-decisions/Ancient-Egyptian-Scribe-Hieroglyphs.jpg
  overlay_filter: 0.5
  teaser: assets/architecture-decisions/scribes-at-work.jpg
---
Please, not this is **work in progress**

* Do not remove this line (it will not be displayed)
{:toc}

# What is it
An architecture decision (AD) is a software design choice that addresses a significant requirement.
From [here](https://github.com/joelparkerhenderson/architecture-decision-record#what-is-an-architecture-decision-record)

During project team makes multiple ADs.
Each decision is made at some point in time and within specific context - project history, current functional requirements (i.e. set of user stories), set of non-functional requirements, product vision, clients in pipe etc. This context is subject to change in time.

# Track it
[ADR](https://github.com/joelparkerhenderson/architecture-decision-record) is common approach to track AD 

Very good example of ADR usage - *A Case Study - Konstantin Kudryashov - DDD Europe 2020*

{% include video id="x5YmBevdjVg" provider="youtube" %}

# Evaluate it
What does it mean to evaluate AD? When should it be done?
![](assets/architecture-decisions/architecture-decisions-process.png)

# Quadrants

![architecture decisions - effort vs business value](assets/architecture-decisions/architecture-decisions-quadrant.png)

## Magic
The art of gathering low hanging fruits.
Bringing in some new app, lib, pattern that proved to work in similar domains, but it’s unfamiliar to project team.
As an example check article [Sql is not everything you need](/not-only-sql)

## Safe play
**No pain no gain** or **agile trap** quadrant.
How is it *an agile trap*? 
All ADs that are made with only looking at current needs of the project. Is it bad – no. But this kind of decision making will probably make you start *v2.0* of your app much sooner than expected. Reason - no **platform vision** in architecture.

## Vision
Hardest one, because it’s easy to fail into *overengineering*. Example - migration from Docker Swarm into Kubernetes. Unless product really profits from this migration in near future (6-12 months) this might be an overkill.

## Overengineering
Nothing wrong with this quadrant, unless all decisions end up here.

# Multiple ADs, not ONE
![multiple architecture decisions](assets/architecture-decisions/architecture-decisions-portfolio.png)


# Value over time
Good example is AD from *vision* quadrant. Rarely you can put them right in that quadrant from the very beginning. Initially these decisions tend to move to *overengineering* quadrant first, and if team is lucky and persistent, it will eventually move to *vision*.

![architecture decisions value evolution over time](assets/architecture-decisions/architecture-decisions-value-evolution-over-time.png)

# Strategy
Let's recap 
- Safe play is obvious choice for most companies. But that's mid-term trap.
- To get some magic – networking is an answer, or bringing in some external people with different domain expertise.
- Vision – requires both technical and product components. 

Team should work with ADs as with any portfolio using **risk-reward** estimation approach, i.e. in general, accept some risk of **overengineering** but manage it.

![risk reward](assets/architecture-decisions/portfolio-risk-reward.jpeg)

Managing risk of *overengineering* means
- mistakes will happen, but they are allowed
- reevaluate AD often
- be transparent and honest with team


# Guesstimate
Do not underestimate element of luck in decision making - team operates with incomplete information in undetermined world .