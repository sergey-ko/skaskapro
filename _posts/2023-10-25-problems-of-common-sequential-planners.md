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

>**Disclaimer #1:** The insights shared in this article are based on experiments conducted with Semantic Kernel (version < 1.0). While Langchain exhibits similar challenges, I have not delved deeply into its workings.

>**Disclaimer #2:** The crux of this discussion revolves around devising a plan, rather than its execution.

>**Disclaimer #3:** Concepts such as CoT, ToT, GoT, etc., are not pertinent to this discussion.

## The Sequential Planner Flow

An overview of the current implementation of the sequential planner:

**Given:**
- A goal expressed as a string.
- A list of available functions that the planner can interpret (SK => plugins).

**Flow:**
- Extract a concise list of functions that may be relevant for the task at hand.
- Formulate a prompt encompassing:
  - Descriptions of the selected functions.
  - Directions to craft a plan.
  - The specified goal.
- Engage the LLM with the crafted prompt to obtain the plan.

#### Plan Example:

Given the objective: "Summarize an input, translate it to French, and e-mail it to John Doe", the following plan was devised:

```
Steps:
  - SummarizePlugin.Summarize input='$INPUT' => SUMMARY
  - WriterPlugin.Translate input='$SUMMARY' => TRANSLATED_SUMMARY
  - email.GetEmailAddress input='John Doe' => EMAIL_ADDRESS
  - email.SendEmail input='$TRANSLATED_SUMMARY' email_address='$EMAIL_ADDRESS'
```

## Limitations of the Current Approach

While this methodology suffices for rudimentary tasks with concise plans, it falters when addressing more intricate challenges. Some of the pitfalls include:

- Irrespective of the prompt instructions, the generated plan may inadvertently employ collections as variables.
- The structure of the final solution remains ambiguous. Merely augmenting instructions is ineffective, given the absence of a verification mechanism.
- Should a function crucial for achieving the goal be absent, the invoking system remains oblivious.
- Similarly, if supplementary information is essential for goal accomplishment, the system remains uninformed.

## A Glimmer of Hope

Firstly, LLMs at the caliber of GPT-4 are equipped to devise algorithms to tackle almost any challenge (e.g., the 12 tasks delineated in the [ARC report](https://evals.alignment.org/blog/2023-08-01-new-report/)). This includes the ability to decompose a task into more manageable sub-tasks. While the resultant algorithm may not always be optimal, leveraging multiple generations (with a non-zero temperature) could pave the way for satisfactory outcomes.

## Proposed Solution

The aspiration is to develop a planner capable of:
- Ensuring that the final result adheres to the defined structure and meets acceptance criteria.
- Informing the invoking system in case certain information or functionality is lacking.
- Employing a top-down strategy, further dissecting tasks as needed.
- Seamlessly integrating with collections/lists and other data structures.

