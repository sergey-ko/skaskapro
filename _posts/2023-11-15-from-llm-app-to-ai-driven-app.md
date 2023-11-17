---
title:  "From LLM-app to AI/LLM-drive-app"
date:   2023-11-15 01:00:56 +0700
categories: ai
tags: ai
tagline: ""
header:
  overlay_image: assets/llm-app/overlay.jpg
  overlay_filter: 0.5
  teaser: assets/llm-app/teaser.jpg
---

> [WiP]

# Introduction

In this article, we delve into the evolving landscape of AI-driven applications, particularly focusing on Large Language Models (LLMs). Let's begin by defining key terms:

- **LLM App**: An application that primarily utilizes Large Language Models for its core functionality.
- **AI-Driven App**: An alternative approach to app development that heavily relies on AI technologies, including but not limited to LLMs.

Our journey starts with an analysis of David Shapiro's demonstration of a medical application, specifically the [**Medical Intake**](https://github.com/daveshap/Medical_Intake) app. This type of application is what we refer to as an LLM-app. Yes, it's not a production grade app, but i a good starting point for our jorney.

![Medical intake app](/assets/llm-app/intake-app.jpg)

To understand example better - let's split LLM interaction logic (aka prompt engineering) and control logic (aka program flow). While prompt in this app maybe very curated, control logic is very simple:
- chat with patient and summarize chat into _notes_
- generate multiple documents based on these _notes_

Our goal is to show how to enhnace _control logic_ with domain expertise and make app more flexible and reliable.

# Steps to Transition from an Initial LLM-App to an AI-Driven App

1. **LLM App**: The foundational stage where the app is primarily driven by LLM capabilities.
2. **Modularization**: Break down the intake process into distinct LLM-activities. This modular approach facilitates structural prompting, response verification, and efficient testing.
3. **Workflow**: Implementing a workflow engine can significantly enhance the app by introducing monitoring, reliability, and other benefits associated with workflow/business process management systems (WF/BPMS). It also simplifies the addition of new steps.
4. **Evolution to Case Management**: Drawing from over a decade of experience in BPMS integration, it's observed that workflows tend to become more complex and numerous over time, often evolving into a Case Management System. In our context, the "case" is the intake process, typically comprising various stages and actions (akin to steps in the workflow approach).
5. **LLM-Driven Decision Making**: The final evolutionary step involves empowering the LLM to oversee case management decisions. While individual actions/steps continue to involve LLM interactions, a new overarching LLM is introduced to orchestrate the entire process.


## LLM app 

![LLM app](/assets/llm-app/llm-app.jpg)

## Modularization
![Extracted steps app](/assets/llm-app/extracted-steps-app.jpg)

## Workflow
![Worflow app](/assets/llm-app/wf-engine-driven-app.jpg)

## Adaptive case management
![ACM app](/assets/llm-app/acm-driven-app.jpg)

## AI
![AI app](/assets/llm-app/ai-driven-app.jpg)




# AI-driven app architecture

Key blocks of an AI-Driven App

- **Core LLM**: The same LLM used in the initial app is retained for individual activities/steps. It's now enhanced with verification mechanisms, structured prompts, and other techniques to bolster the reliability of each step. This component remains flexible and configurable.
- **Step Types**: Incorporate more diverse steps, such as consulting with experts or retrieving and pre-analyzing data from external systems.
- **LLM for Step Management**: Introduce a new LLM layer to determine the sequence of step execution. Initially, this might be a simple expert system, but with added configurability and external rule storage, it can evolve alongside the system.
- **Hardcoded Framework**: A foundational structure that encapsulates the concept of steps/actions and their interaction with the executor-LLM.
- **Rules**: Pieces of knowledge that can be used to improve AI performance in complex situations
- **Structured Document Storage**: As alternative to raw vertor storage


# Conclusion

Demis Hassabis, in an interview, once remarked that the more tasks they could delegate to neural networks, the better the outcomes. While this is a compelling argument, not everyone has access to high-end computational resources like the _GH200_ clusters. Therefore, exploring alternative methods and hybrid approaches becomes crucial in the development of efficient and effective AI-driven applications.
