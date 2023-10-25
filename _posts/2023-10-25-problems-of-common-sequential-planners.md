---
title:  "Sequential planner flaws"
date:   2023-10-24 10:00:56 +0500
categories: ai
tags: ai
tagline: ""
header:
  overlay_image: assets/sequential-planners/robot-planner-3-wide.jpg
  overlay_filter: 0.5
  teaser: assets/sequential-planners/robot-planner-2-sm.jpg
---

>Disclaimer #1: The following observations are derived from experiments with Semantic Kernel (version < 1.0). Although Langchain has similar issues, I haven't delved deeply into it.

>Disclaimer #2: The primary focus of this discussion is on formulating a plan, not its implementation.

>Disclaimer #3: Concepts like CoT, ToT, GoT, and the like are not relevant in this context.

## Sequential planner flow

Current implementation of sequential planner

__Given__
- goal as string
- list of available functions in the form planner can understand (SK => plugins)

__Flow__
- extract smaller list of functions that might be usefull for specified task
- construct a prompt that includes
  - list of functions descriptions
  - instructions to construct plan
  - goal
- call LLM with constructed prmpt to get the plan

#### plan example
For a given goal: Summarize an input, translate to french, and e-mail to John Doe. Following plan was created

```
 Steps:
  - SummarizePlugin.Summarize input='$INPUT' => SUMMARY
  - WriterPlugin.Translate input='$SUMMARY' => TRANSLATED_SUMMARY
  - email.GetEmailAddress input='John Doe' => EMAIL_ADDRESS
  - email.SendEmail input='$TRANSLATED_SUMMARY' email_address='$EMAIL_ADDRESS'
```


## Downsides of such implementation
For simple tasks with small plans current approach works. All the fun starts when we try bigger and more complicated problems.

- despite promt instruction, generated plan might use collections as variables
- final solution structure is undefined. additional instructions does not help since there is no verification
- if function required to fullfill the goal is missing, calling system would never know about it
- if additional information is required to fullfill the goal, calling system would never know about it

## There is light

First, GPT4 level LLM can provide algorithm to solve almost any task (i.e. 12 tasks mentioned in [ARC report](https://evals.alignment.org/blog/2023-08-01-new-report/)). I.e. split task into smaller ones.
Yes, provided algorithm might not be ideal, but with the help of multiple generations (with non-zero temperture) we might achive proper results.


## Idea

Create planner that can 
- confirm that final result structure and acceptance criterias are met
- respond to calling system if some information or functionality is missing 
- use top to buttom approach and split tasks into smaller one if necessary
- work with collections/lists and other data structures

