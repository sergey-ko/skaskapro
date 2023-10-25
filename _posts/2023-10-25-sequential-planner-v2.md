---
title:  "Sequential planner v2"
date:   2023-10-25 10:00:56 +0500
categories: ai
tags: ai
tagline: ""
header:
  overlay_image: assets/sequential-planners/robot-planner-3-wide.jpg
  overlay_filter: 0.5
  teaser: assets/sequential-planners/robot-planner-2-sm.jpg
---

This is part 2 ([part 1](/problems-of-common-sequential-planners))

## Initial idea for plan generation

Create planner that can 
- confirm that final result structure and acceptance criterias are met
- respond to calling system if some information or functionality is missing 
- use top to buttom approach and split tasks into smaller one if necessary
- work with collections/lists and other data structures


## Implementation
Main points
- user or invoking system provides information necessary to formulate extended version
- planner consists of 2 modules - task manager that interacts with user/system and planner that creates plan and interact with task manager

![seqential planner v2](/assets/sequential-planners/planner_v2.png)


### Extending task description
 Task description that is passed to planner need to have not olny task text but also
 - high level values for planner (i.e. mission, project context)
 - input information description
 - resulting documents description
 - acceptance criterias

 This extended description can be either result of interaction with the user/system or generated from project description and documentation, common sense etc.

### Hierarchical task splitting

Start of the algorithm is almost the same as for conventional sequential planner. Difference is extended task description, but we can ignore it for now.

__Given__
- task as string
- extended task description
- list of available functions in the form planner can understand

__Algorithm__
- try to solve task using conventional planner and existing functions. if solved => __hurra! done__
- ask planner to split task into several (not many 5-10) stages with detailed description (same as extended description for initial task) on each step
- [recursion]
  - for each stage try to resolve it by conventional planner or by splitting into smaller stages
  - if recurion work => fine!
  - if not - dig down till some level.

__Key points__
- keep records of all explored steps (tree/graph of hypothesys)
- be creative and try splitting into stages multiple times (i.e. high __temperature__)
- distinguish between 2 problem for solution not found (stage can not be rresolved with conventional planners)
  - can not apply conventional planner due to lack of functions
  - additional information/document is required

Main difference of described approach from __[X]_of_Thoughts__ approaches (CoT, ToT, GoT etc.) is that we are ok to introduce non-defined steps and define them later, instead of doing search only in the space of availble functions.

### Example A: simple case => success plan
Below is diagram describing simple case that is identical to common sequential planner
- task is successfully split into several activities/subtasks
- each activity is mapped to exisiting skill from given skillset
- input document mapped to input of *activity A* (skill A)
- output document is output from last *activity C* (skill C)

![simple case plan](/assets/sequential-planners/simple_case_plan.png)


### Example B: simple case => problematic plan
This is more complex case 
- planner was unabled to map initial task into sequence of skills
- planner splitted initial task into high level activities
  - activity #1 and activity #2 were sucessfully mapped into sequence of skills
  - but there was a problem with activity #3 - it requred additional information (document X)
- though planner tried to create plan for several times , __document X__ was always required 
- this request for __document X__ passed to __task manager__ module


![bad planning](/assets/sequential-planners/multilevel_case_plan.png)


## Conclusion
High level description of algorithm for advanced planning was given and initial ideas implemented.