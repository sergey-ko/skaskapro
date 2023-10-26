---
title:  "Workforce agent"
date:   2023-10-26 01:00:56 +0700
categories: ai
tags: ai
tagline: ""
header:
  overlay_image: assets/sequential-planners/robot-planner-3-wide.jpg
  overlay_filter: 0.5
  teaser: assets/sequential-planners/robot-planner-2-sm.jpg
---

Cosider this part 3 of "Sequential planner' series ([part 1](/problems-of-common-sequential-planners), [part 2](/sequential-planner-v2))

## Idea

Imagine having unlimited access to __remote__ mid and junior level knowledge workers. The only downside is that they do not Zoom. Otherwise
- they know how to use Jira and Confluence
- they can work 24*7
- they track thier work and validate results
- they asks for help 
- ... and so on

### interaction scenario

Example of possible interaction scenraio
- User have a chat with **manager agent** (MA) and formulate a task
- MA sets task in Jira and assing it to proper **worker agent** (WA)
- WA creates a plan to execute a task, asks clarifing questions and/or additional documents. Plan can be in the form of sub tasks in Jira.
- After plan is created WA starts to execute it, while updating each sub task status
- After all subtasks are executed and task is finished, MA runs verification/validation task and if everything is ok, sets status of initial task to *done*


## Current state of agent development

### Simple
Agents viewed as remote procedure call not as planning and executing entity. Sometime even orchestrated from outside (i.e. check __agentprotocol__)

### Planners
Please check [part 1](/problems-of-common-sequential-planners) and [part 2](/sequential-planner-v2) to understand flaws of current planners and ways to evolve them.

### Reasoning
CoT, ToT, GoT - these approached aimed more at reasoning, not at cooperative task solving. Though they can definetly be used as _low level_ approaches.

## Main hint - workflows engines analogy
Yes, workflows are something that has to go on and on for multiple times. But it's main analogy to understand task execution. Check something like Camunda, Nintex,  Azure Logic Apps etc..
Think of task setup as configuring workflow 
- internal variables
- subprcesses
- external system update
- etc.
Task execution is like executing such workflow. But only once.


## Missing ingredients

4 main ingredients that are missing in current SOTA agent developments
- advanced task planning approach, including hierarchical task split and ask-plan-execute approach
- flexibilities in terms of multiple kernels/LLMS, complex internal (to task) data types (entities, collections)
- resilience as external task and subtasks states and statuses
- integration with tasks management systems, wikis, document portals

![new agent core](/assets/sequential-planners/new_core.png)


## Agent Core

### 3 modules

Agent core functionality can be split into 3 modules
- manager 
  - interacts with user or high level system
  - main goal is to create detailed description of task
- planner
  - using detailed task description tries to build a plan for execution
  - can ask manager for additional info and report problems if task plan can not be created due to lack of skills orinformation
  - provides search tree together with execution plan, so that executor have some infor in case of problems during plan execution
  - plan is posted to task management system (TMS)
- executor
  - executes plan by either using skill directly or with help of _simple agents_
  - during execution tasks statuses are updated in TMS, some artifacts can be created in wikis, portals, code repositories

### Planning execution
Treat agent as junior dev - do not expect perfect knowledge of your domain and perfection in anything. 
Let agent to formulate what and how it plan to achieve required result (check __Hierarchical task split__).
Hierachical planner can return either plan to execute or one of 2 errors - lack of functions and lack of information. 
Lack of additional information or functionality can be treated as call for help.

### Tasks, wikis etc.

While common approach (exept for MemGPT) is to keep all execution log in context, idea is to use wiki and/or task description to store intemediary information.
Reasons
- this is key for parrallel execution and multiple agents working on subtasks
- tracking of agents activities is simple due to common solutions task management and wikis

To achieve this agents need to have ideas on tasks, subtasks, thier relations, statuses etc. and wiki structure. Reasoning behind this is that differect teams run projects in different ways.

## Future work
Future work is simple - implement __Core__ for Wrokforce Agents.