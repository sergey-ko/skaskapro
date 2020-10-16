---
title:  "Know your rules"
date:   2020-01-02 11:45:56 +0700
categories: bpms
tags: k2, bpms, rules engine
header:
  image: /assets/know-your-rules/know the rules sm.jpg
---
Two-stage 'expenses approval' is used as example to show how and why rules engine can be used with BPMS. Process's dynamics is complete fake and used only for demo purposes.

# Step #0. Basic process

![basic approval workflow](\assets\know-your-rules\wf-1.png)

Usually result of first version of process deployed to production looks very simple. In this case
* Person A - manager of person submitted request.
* Person B - accountant.

# Step #1. Add more approval rules
After using process for some (very short) time person B understands that it does not want to approve anything below $100. He can supports his decision by providing 'processes bottleneck' report from BPMS.

![more rules to workflow](\assets\know-your-rules\wf-2.png)

# Step #2. Add more rules...again
Very soon 2 more new requirements come
* top management requests need not be approved
* amount of request not to approved by person B differ by requester's department - for salesforce amount is 200 and 100 for other

![even more rules to workflow](\assets\know-your-rules\wf-3.png)

# Step #3. Collapse it with rules engine
Process starts looking like a mess and understanding that new requirements will come soon is clear. Rules engine is introduces.Two rules added - one for 'approval required' and one for 'amount to approve'. To get this information from rules engine 2 service calls added respectively.

![add rules engineto workflow](\assets\know-your-rules\wf-4.png)

As result process is simplified - less branching. Most of changes can be done in rules, without changing process. 

# Step #4. Add approval process to change rules
Since rules engine holds some 'policies' why not add process to change these policies - this helps keep them under control and completely takes 'change policy activities' off IT department's shoulders.

![approve workflow rules with workflow](\assets\know-your-rules\wf-5.png)

# Conclusion
Business processes are alive and even back-office processes are subject to changes, not to speak about primary business process that is changing constantly. BPMS helps keep IT solutions in tact with business requirements and reality. Rules engines help bring hardcoded business rules onto surface and manage them. Proper balance between tasks in BPMS and rules in rules engine helps keep processes clear and rules meaningful.
