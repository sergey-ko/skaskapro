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


Alternative title: **Exploring the Advanced Skill Spectrum of LLM-Powered Chatbots**

## Introduction

In the realm of artificial intelligence, language model (LLM)-powered chatbots represent a breakthrough in interactive technology. While base skills like web browsing are common in most chatbots, it's the enhancement and core transformation skills that are pushing the boundaries of what these digital assistants can achieve.

![3 skills layers](/assets/3-skill-layers/3_layers_of_skills.png)

## Enhancing skills

There is no reason not to enhance base skill set using chat interface, project like **`GptEngineer`** prove that this is possible to write programs using only chat like interaction between user and LLM. Skills at this layer may include not only creating new base skills from scratch, but also updating existing and combining them into more powerfull complex base skills.
Assistan chat here is an alternative to common IDE for creating skills (that are nothing but pieces of code).

![enhancement skills](/assets/3-skill-layers/enhancement_skills.png)

At this layer we first meet ability for the skill to improve itself, with the help of use for now.
in enterprise application this layer should probably be accessed by power users and regular developers.

## Changine 'core'

Taking a dive a little deeper we can change the underlaying interaction layer between app and LLM, so called [LLM programs](https://arxiv.org/abs/2305.05364). To make it simple let's take a quick look into teachable agent class of AutoGen project
```python
def consider_memo_storage(self, comment):
        """Decides whether to store something from one user comment in the DB."""
        # Check for a problem-solution pair.
        response = self.analyze(
            comment,
            "Does any part of the TEXT ask the agent to perform a task or solve a problem? Answer with just one word, yes or no.",
        )
        if "yes" in response.lower():
            # Can we extract advice?
            advice = self.analyze(
                comment,
                "Briefly copy any advice from the TEXT that may be useful for a similar but different task in the future. But if no advice is present, just respond with 'none'.",
            )
        ...            
```
in this method we first call LLM and them try to understand it response in 'formal' terms 'yes'/'no'. This is simplest example of LLM-program - call and response interpretation.
This method 'consider_memo_storage` is about 50 lines of code, and while it's a good code at the first glance, we can not be sure that it operates 'optimally' according to our requirements. Current state of this code block is just a hypothesys. 
During usage of app we can collect logs for this method in the form of 
- method implementation (keeping in mind that code is subject to change/evolve)
- input
- output


![core transformation skill](/assets/3-skill-layers/core_transformation_skill.png)

At this layer we continue our jorner into the realm of self-improving LLM-apps.


## Conclusion

The advanced skill spectrum of LLM-powered chatbots reveals their potential beyond basic tasks. Enhancement skills allow chatbots to evolve their capabilities through interactive interfaces, moving beyond traditional programming. Core transformation skills delve deeper, optimizing chatbot core. 

