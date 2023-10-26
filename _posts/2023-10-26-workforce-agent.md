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

**Consider** this as part 3 of the "Sequential Planner" series: [part 1](/problems-of-common-sequential-planners), [part 2](/sequential-planner-v2).

## Idea

Imagine having unlimited access to **remote** mid and junior-level knowledge workers. The only limitation? They don't use Zoom. However:
- They are proficient with Jira and Confluence.
- They are available 24/7.
- They meticulously track their work and validate outcomes.
- They seek assistance when needed.
- ... and much more.

### Interaction Scenario

Here's an illustrative interaction scenario:
- A user converses with a **manager agent** (MA) to define a task.
- The MA logs the task in Jira and assigns it to the appropriate **worker agent** (WA).
- The WA drafts a strategy to accomplish the task, seeking clarifications or supplementary documents if necessary. This plan might manifest as sub-tasks within Jira.
- Once the plan is set, the WA commences its execution, continually updating the status of each sub-task.
- After completing all sub-tasks, the MA conducts a verification/validation. If all criteria are met, the status of the primary task is marked as *done*.

## Current State of Agent Development

### Simple
Agents are often perceived as mere remote procedure calls rather than entities capable of planning and execution. Sometimes, they're even externally orchestrated (e.g., check **agentprotocol**).

### Planners
To grasp the limitations of current planners and potential improvements, refer to [part 1](/problems-of-common-sequential-planners) and [part 2](/sequential-planner-v2).

### Reasoning
CoT, ToT, GoT – these methods focus predominantly on reasoning, not on collaborative task resolution. Nonetheless, they can certainly serve as _low-level_ approaches.

## Main Hint: Workflow Engines Analogy

Indeed, workflows are repetitive processes. However, they offer a prime analogy for understanding task execution. Explore platforms like Camunda, Nintex, Azure Logic Apps, etc. Envision task setup akin to configuring a workflow, accounting for:
- Internal variables.
- Sub-processes.
- Updates to external systems.
- And more.
The task execution can be likened to running such a workflow – but just once.

## Missing Ingredients

There are four pivotal components currently absent in state-of-the-art agent developments:
- An innovative task planning methodology, incorporating hierarchical task decomposition and the ask-plan-execute strategy.
- Flexibility concerning multiple kernels/LLMS and intricate internal data types (entities, collections).
- Resilience regarding external task and sub-task states and statuses.
- Seamless integration with task management systems, wikis, and document portals.

![New Agent Core](/assets/sequential-planners/new_core.png)

## Agent Core

### Three Modules

The core functionality of an agent can be divided into three modules:
- **Manager**: 
  - Engages with users or high-level systems.
  - Primarily aims to draft a detailed task description.
- **Planner**:
  - Utilizes the detailed task description to devise an execution plan.
  - Can seek further information from the manager or report issues if the task plan is unfeasible due to insufficient skills or data.
  - Delivers a search tree alongside the execution plan, offering insights to the executor in case of execution issues.
  - The plan is uploaded to the task management system (TMS).
- **Executor**:
  - Carries out the plan either autonomously or with the assistance of _simple agents_.
  - Updates task statuses in the TMS during execution. Some artifacts may also be generated in wikis, portals, or code repositories.

### Planning Execution

Approach the agent as you would a junior developer: don't anticipate exhaustive domain knowledge or flawless execution. Allow the agent to outline its approach to achieving the desired outcome (see **Hierarchical task split**). A hierarchical planner can either produce an executable plan or one of two errors: lack of functions or lack of information. These inadequacies can be interpreted as calls for assistance.

### Tasks, Wikis, etc.

While the conventional approach (except for MemGPT) retains the entire execution log in context, the proposition here is to use a wiki and/or task description to house intermediary data. The motivations are:
- It's pivotal for parallel execution and multiple agents working on sub-tasks.
- Agent activities are easily traceable, thanks to standard task management solutions and wikis.

For this to be feasible, agents must understand tasks, sub-tasks, their interrelations, statuses, etc., as well as the structure of the wiki. This understanding is essential as different teams manage projects in varied ways.

## Future Work

The way forward is clear: develop the **Core** for Workforce Agents.