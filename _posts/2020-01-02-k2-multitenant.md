---
layout: post
title:  "K2 multitenant SaaS"
date:   2020-01-02 11:45:56 +0700
categories: k2
tags: k2, saas, multitenant
---
This is simplified case of real project, which included several multi-tenant systems we had to integrate and put some business process on top of them.

# Situation
Group of companies with shared processes.
Each company has its own QBO account.
Not all employees have access to all companies

![multitenant access goal](/assets/k2-multitenant/multitenant goal 2.png)

Solution architecture should let us create one form for any case (for example ‘Employee’ entity) not multiple forms (one for each company). Same for workflows – one SmartObject to ‘rule them all’ one entity of all companies, not multiple SmartObjects for same entity. Keep it simple, maintainable and extensible.

![quickbooks switch company](/assets/k2-multitenant/qbo switch company.jpg)

Ability to switch companies – QBO App like experience and adding new companies should not be a big deal.

# Problem
First option to consider is to use K2 functionality to access external systems using OAuth2. In this case, for each account (company) you have to create instances for appropriate SmartBroker. Then create SmartObject for each of these ServiceBroker Instances. Finally, you face question how to use all these SmartObjects in one form or in the same step of K2 Workflow.

![k2 form and mulltiple brokers](/assets/k2-multitenant/multitenant problem.png)

# Solution
Proposed solution includes lambda-k framework that helps in scenarios like this. Main components of the solution
* OAuth handler - does exactly what K2 does with OAuth2 – stores access info and keeps keys actual
* Custom service broker to access QBO
* Company info storage (logo, title, employee access etc.)
* K2 SmartForms header view to be used on all forms that require access to ‘multiple-company’ info

![k2 multitenant architecture](/assets/k2-multitenant/multitenant architecture 2.png)

Control flow when user opens form is straightforward - header view is loaded first

* Checks if there is an active organization id stored in browser or session
* Sends this info along with user account id to backend to get correct active organization id that user has access to
* Stores validated organization id as view’s parameter
* Other form’s views are loaded using organization id from header-view. Calls to QBO flow through custom service broker that uses actualized OAuth access info  

# Header view
This view contains following controls
* Company logo or name, so that user is always aware what company he is working on
* Custom control to store active/current company’s id
* Html control with JavaScript to keep this view always on top of the page (above other views and form’s tabs)

![k2 multitenant header](/assets/k2-multitenant/multitenant header.png)

When user wants to change company, he clicks logo image and selects company from list of companies he has access to.

# Conclusion
Client received solution for this multi-company scenario that both maintainable and easily extensible for new forms and companies. Same architecture is applicable in integrational scenarios with other systems/SaaS that use OAuth authentication.

![k2 multitenant form example](/assets/k2-multitenant/k2 multitenant form example.jpg)