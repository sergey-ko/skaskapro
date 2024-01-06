---
title:  "3 Layers of skills in extendable and adaptive LLM-chatbot"
date:   2024-01-06 02:00:56 +0700
categories: ai
tags: ai
tagline: ""
header:
  overlay_image: assets/3-skill-layers/overlay.jpg
  overlay_filter: 0.5
  teaser: assets/3-skill-layers/teaser.jpg
---

## Introduction

In the dynamic world of artificial intelligence, Language Model (LLM)-powered chatbots stand at the forefront of interactive technology. While foundational skills such as web browsing are commonplace in many chatbots, it is the realms of enhancement and core transformation skills that are truly revolutionizing the capabilities of these digital assistants.

![3 skills layers](/assets/3-skill-layers/3_layers_of_skills.png)

## Enhancing Skills

Enhancing the base skill set via a chat interface is now a tangible reality. Innovations like **`GptEngineer`** demonstrate the feasibility of programming through chat-like interactions between the user and the LLM. This layer encompasses more than just crafting new base skills from the ground up; it involves refining and amalgamating existing skills into sophisticated, multifaceted tools. Here, the chat interface serves as a unique alternative to conventional Integrated Development Environments (IDEs), facilitating the creation and modification of code.

![enhancement skills](/assets/3-skill-layers/enhancement_skills.png)

This layer introduces the concept of self-improving skills, initially with user assistance. In a business context, these advanced capabilities are ideally suited for power users and developers.

## Changing the 'Core'

Delving deeper, we can modify the foundational interaction layer between the application and the LLM, known as [LLM programs](https://arxiv.org/abs/2305.05364). For instance, consider the `consider_memo_storage` method from the AutoGen project:
```python
def consider_memo_storage(self, comment):
        """Determines if a user comment should be stored in the database."""
        # Analyzing for a problem-solution context.
        response = self.analyze(
            comment,
            "Does any part of the TEXT ask the agent to perform a task or solve a problem? Answer with just one word, yes or no.",
        )
        if "yes" in response.lower():
            # Extracting actionable advice.
            advice = self.analyze(
                comment,
                "Briefly copy any advice from the TEXT that may be useful for a similar but different task in the future. If no advice is present, respond with 'none'.",
            )
        ...            
```
In this method, the LLM is initially consulted, followed by an interpretation of its response in binary terms ('yes'/'no'). This is a basic form of an LLM program, involving a call-and-response mechanism. The `consider_memo_storage` method, spanning roughly 50 lines, represents well-structured code, yet its operational efficiency and alignment with our objectives remain hypotheses subject to verification.

![core transformation skill](/assets/3-skill-layers/core_transformation_skill.png)

At this layer, we extend our exploration into self-improving LLM applications. By analyzing logs comprising method implementations, inputs, and outputs, we can iteratively refine the LLM program, potentially leveraging [advanced LLMs](https://arxiv.org/abs/2309.03409) for optimization.

## Conclusion

The exploration of the advanced skill spectrum in LLM-powered chatbots uncovers a world where these tools transcend basic functions. Enhancement skills empower chatbots to grow and adapt through interactive interfaces, stepping beyond the bounds of traditional programming. Core transformation skills take this further, honing the very essence of chatbot functionality. A key consideration emerges: determining which aspects of the application should remain constant throughout its lifecycle, amidst the continuous evolution of its capabilities. This question underscores the intricate balance between innovation and stability in the development of intelligent chatbot applications.