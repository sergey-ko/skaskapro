---
title:  "Export costs from Azure"
date:   2020-10-24 09:00:00 +0700
categories: azure
tags: azure
tagline: ""
header:
  overlay_image: assets/azure-costs/cost-management-hero.jpg
  overlay_filter: 0.5
  teaser: assets/azure-costs/cost-management-teaser.jpg
---

Suppose your organization uses more than one cloud provider - Azure, AWS and Atlassian for different functions. Financial department desperately needs more advanced cost analysis by teams/projects/etc. 
Requirement is simple - make costs data accessible in some corporate BI (PowerBI, Tableau). In this article will take a look at different aproaches for exporting Azure costs.
First resource you should get acquainted is [Azure Cost Management](https://azure.microsoft.com/en-us/services/cost-management/)

# Manual export

![manual export](\assets\azure-costs\manual export dataflow.png)

Export costs manually for last period from all subscriptions using *Cost management + Biling*. 
Go to *Cost analysis* – set grouping options, periods and granularity. 
Then go to *Download*, select *Excel*, press *Download Data* and you instantly get Excel file with data required.
Having real piece of data lets you start deeper conversation with financial department – what and how to import into BI sources. Some *tagging* and *resources* reorganization might be expected on Azure side.

# Automatic export

![automatic export](\assets\azure-costs\azure cost management scheduled export and import.png)

Once requirements for data are specified in more details and everybody speaks the same language, export of Excel data might be automated.
Go to *Configuration:Exports* and create new data export with periodicity that you need.
Create Azure function import data into data source for BI and run it either periodically or on event based approach (which make more sense). 
To add some resilience *data release* approach can be implemented (read more [here](_posts\2020-10-16-data-release-management.md)).

# Cost management API

![Azure Cost Management API](\assets\azure-costs\azure cost management API.png)

If financial department is hungry for *rich daily data* - [Azure Cost Management REST API](https://docs.microsoft.com/en-us/rest/api/cost-management/) goes to the rescue.
Architecture become a little more complicated, yet rather straight forward.
3 Azure functions connected via ServiceBus are used
1.	Scheduled function. Creates tasks for data extraction for every subscription in the account
2.	Queries data from API and sends it to Bus in single chunk
3.	Ingest data recieved from bus into storage (same function as in *Automatic Export*)

## API POST call example
 
### Url
```
https://management.azure.com/subscriptions/{subscription-id}/
providers/Microsoft.CostManagement/query?api-version=2019-11-01
```

Authentication is required

### Request body 

```json
{
    "type": "Usage",
    "timeframe": "WeekToDate",
    "dataset": {
        "granularity": "Daily",
        "aggregation": {
            "totalCost": {
                "name": "PreTaxCost",
                "function": "Sum"
            }
        },
        "grouping": [
            {
                "type": "Dimension",
                "name": "ResourceGroup"
            },
            {
                "type": "Dimension",
                "name": "ServiceName"
            }
        ]
    }
}
```

### Sample response

```json
{
  "id": "subscriptions/*****/providers/Microsoft.CostManagement/query/******",
  "name": "*****",
  "type": "Microsoft.CostManagement/query",
  "location": null,
  "sku": null,
  "eTag": null,
  "properties": {
    "nextLink": null,
    "columns": [
      {
        "name": "PreTaxCost",
        "type": "Number"
      },
      {
        "name": "UsageDate",
        "type": "Number"
      },
      {
        "name": "ResourceGroup",
        "type": "String"
      },
      {
        "name": "ServiceName",
        "type": "String"
      },
      {
        "name": "Currency",
        "type": "String"
      }
    ],
    "rows": [
      [
        0,
        20201025,
        "*****",
        "azure app service",
        "RUB"
      ],
      [
        0.000075,
        20201025,
        "*****",
        "bandwidth",
        "RUB"
      ],

        .......

    ]
  }
}
```

As you can see response is easy to undestand in ingest into storage.
You can play with API Query [on Microsoft documentation site](https://docs.microsoft.com/en-us/rest/api/cost-management/query/usage)
