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

- **LLM App**: An application that primarily utilizes Large Language Models for its core functionality and simple scripting for interation with LLM
- **AI-Driven App**: An alternative approach to AI app development that also actively uses LLMs both for core functionality and control flow   

Our journey starts with an analysis of David Shapiro's demonstration of a medical application, specifically the [**Medical Intake**](https://github.com/daveshap/Medical_Intake) app. This type of application is what we refer to as an LLM-app. Yes, it's not a production grade app, but a good starting point for our jorney.

![Medical intake app](/assets/llm-app/intake-app.png)

To better grasp this example, we'll distinguish between LLM interaction logic (or prompt engineering) and control logic (program flow). The Medical Intake app, for instance, uses a straightforward control logic:
- Engage in a conversation with a patient and condense the chat into _notes_.
- Use these _notes_ to generate multiple documents.

Our aim is to demonstrate how integrating domain expertise can enhance the _control logic_, making the app more flexible and reliable.


# Steps to Transition from an Initial LLM-App to an AI-Driven App

1. **LLM App**: The basic stage where the app's functionality is primarily driven by LLM capabilities.
2. **Modularization**: Decompose the intake process into discrete LLM-powered modules. This allows for more structured prompting, response verification, and streamlined testing.
3. **Workflow Implementation**: Incorporating a workflow engine can significantly improve the app by adding monitoring, reliability, and other benefits associated with workflow/business process management systems (WF/BPMS). It also facilitates the integration of new processes.
4. **Evolution to Case Management**: Drawing from extensive experience in BPMS integration, it's evident that workflows often become more complex, evolving into Case Management Systems. In this scenario, the "case" is the patient intake process, encompassing various stages and actions.
5. **LLM-Driven Decision Making**: The final step involves enabling the LLM to make case management decisions. While individual steps involve LLM interactions, a master LLM is introduced to orchestrate the entire process.



## LLM app 

![LLM app](/assets/llm-app/llm-app.jpg)

The entire functionality is encapsulated within a single Python script.

## Modularization
![Extracted steps app](/assets/llm-app/extracted-steps-app.jpg)

Evolving into a more manageable and testable structure is a logical next step.

## Workflow
![Worflow app](/assets/llm-app/wf-engine-driven-app.jpg)

Introducing workflow engines or business process management solutions is the natural progression for step orchestration.

## Adaptive case management
![ACM app](/assets/llm-app/acm-driven-app.jpg)

Even though the initial process seems simple, the need for greater flexibility becomes apparent over time. Adaptive Case Management (ACM) addresses this by breaking down processes into smaller units, introducing the concept of a case with its lifecycle, and centralizing human expertise in managing these cases. 

## AI
![AI app](/assets/llm-app/ai-driven-app.jpg)

Replacing the human component in ACM with an LLM, we focus on control flow rather than individual tasks. This LLM determines the next steps based on the current state and available actions. The control flow can be enhanced with expert system-style rules, additional information, or alternative data representations. Crucially, integrating human expertise remains vital for moderating AI behavior, adjusting prompts, and introducing new rules. This requires collaboration between AI specialists and domain experts. 

Check simplified prompt template as below. Given global task, current state (in form of collected documents) and available actions LLM have to come up with next step to perform.


```
{mission}
{goal}

Currently you are in the middle of the {process_name} process.
Below is list of activities you can use to complete the goal and already collected documents from the patient.
===
Activities
{activities}
===
Documents available
{documents}

===
Common happy path is to execute activities in the following order:
{happy_path}
You are free to choose any activities that you think will help you to complete the goal.
Provide name of the activity you want to execute and list of parameters to execute it.
```
This approach to control the flow can further be extented with additional rules (expert system style approach) and other information or other ways to represent same information (i.e. log => actions taken can be presented as results of these actions or rather action description). Additinal flexibility can and should be added by including human expert to moderate AI behavoir by chanaging prompts, introducing new rules etc. Actually there should be 2 experts - one is more AI related with prompt engineering experience, and the second is domain expert () 


# AI-Driven App Architecture

Key components of an AI-Driven App:

- **Core LLM**: Retains the same LLM used in the initial app, now augmented with verification mechanisms and structured prompts to improve step reliability.
- **Step Types**: Introduces a variety of steps, including expert consultations and data retrieval from external systems.
- **LLM for Step Management**: A new LLM layer is implemented to determine the sequence of steps, starting as a simple expert system and evolving with the application.
- **Hardcoded Framework**: A foundational structure that encapsulates the concept of steps/actions and their interaction with the executor-LLM.
- **Rules**: Knowledge segments that enhance AI performance in complex scenarios.
- **Structured Document Storage**: An alternative to raw vector storage, facilitating better data management.






# Conclusion

As Demis Hassabis once remarked, delegating more of the initial task to neural networks leads to superior outcomes. However, not everyone has access to high-end computational resources like the _GH200_ clusters. Therefore, understanding and exploring hybrid methodologies is crucial in developing efficient and effective AI. 
