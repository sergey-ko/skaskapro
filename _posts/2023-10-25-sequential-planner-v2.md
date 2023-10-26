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

This is part 2 ([part 1](/problems-of-common-sequential-planners)).

## Initial Idea for Plan Generation

Create a planner that can:
- Confirm that the final result structure and acceptance criteria are met.
- Respond to the calling system if some information or functionality is missing.
- Use a top-to-bottom approach and split tasks into smaller ones if necessary.
- Work with collections/lists and other data structures.

## Implementation

Main points:
- The user or invoking system provides information necessary to formulate an extended version.
- The planner consists of two modules: a task manager that interacts with the user/system and a planner that creates a plan and interacts with the task manager.

![Sequential Planner v2](/assets/sequential-planners/planner_v2.png)

### Extending Task Description

The task description that is passed to the planner needs to have not only the task text but also:
- High-level values for the planner (i.e., mission, project context).
- Input information description.
- Resulting documents description.
- Acceptance criteria.

This extended description can be either the result of interaction with the user/system or generated from project description and documentation, common sense, etc.

### Hierarchical Task Splitting

The start of the algorithm is almost the same as for a conventional sequential planner. The difference is the extended task description, but we can ignore it for now.

__Given__:
- Task as a string.
- Extended task description.
- List of available functions in the form the planner can understand.

__Algorithm__:
- Try to solve the task using a conventional planner and existing functions. If solved => __hurrah! Done__.
- Ask the planner to split the task into several (not many 5-10) stages with a detailed description (same as the extended description for the initial task) on each step.
- [Recursion]
  - For each stage, try to resolve it by a conventional planner or by splitting it into smaller stages.
  - If recursion works => fine!
  - If not, dig down till some level.

__Key points__:
- Keep records of all explored steps (tree/graph of hypotheses).
- Be creative and try splitting into stages multiple times (i.e., high __temperature__).
- Distinguish between two problems for a solution not found (stage cannot be resolved with conventional planners):
  - Cannot apply conventional planner due to a lack of functions.
  - Additional information/document is required.

The main difference of the described approach from __[X]_of_Thoughts__ approaches (CoT, ToT, GoT, etc.) is that we are okay to introduce non-defined steps and define them later, instead of doing a search only in the space of available functions.

![simplified version of hierarchical task split algorithm](/assets/sequential-planners/hierarchical_task_split.png)

### Example A: Simple Case => Success Plan

Below is a diagram describing a simple case that is identical to a common sequential planner:
- The task is successfully split into several activities/subtasks.
- Each activity is mapped to an existing skill from the given skillset.
- The input document is mapped to the input of *activity A* (skill A).
- The output document is the output from the last *activity C* (skill C).

![Simple Case Plan](/assets/sequential-planners/simple_case_plan.png)

### Example B: Simple Case => Problematic Plan

This is a more complex case:
- The planner was unable to map the initial task into a sequence of skills.
- The planner split the initial task into high-level activities.
  - Activity #1 and activity #2 were successfully mapped into a sequence of skills.
  - But there was a problem with activity #3 - it required additional information (document X).
- Though the planner tried to create a plan several times, __document X__ was always required.
- This request for __document X__ was passed to the __task manager__ module.

![Bad Planning](/assets/sequential-planners/multilevel_case_plan.png)

## Conclusion

A high-level description of the algorithm for advanced planning was given, and initial ideas were implemented.

