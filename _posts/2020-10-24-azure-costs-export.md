---
title:  "Export costs from Azure"
date:   2020-10-24 09:00:00 +0700
categories: azure
tags: azure
tagline: ""
header:  
  image: assets/azure-costs/cost-management-hero.jpg

---

# Cloud cost management
Suppose your organization uses more than one cloud resource (Azure, Atlassian) and financial department desperately needs more advanced cost analysis by teams/projects/etc. 
Requirement is to make azure costs data accessible in some corporate BI (PowerBI, Tableau). This can be easily achived with [Azure Cost Management](https://azure.microsoft.com/en-us/services/cost-management/)

# Manual export

![manual export](\assets\azure-costs\manual export dataflow.png)

Export costs manually for last period from all subscriptions using “Cost management + Biling“. 
Go to “Cost analysis” – set grouping options, periods and granularity. 
Then go to “Download”, select “Excel”, press “Download Data” and you instantly got excel file with data required.
This step lets you start deeper conversation with financial department – what and how to import into BI sources.

# Automatic export

![automatic export](\assets\azure-costs\azure cost management scheduled export and import.png)

Once requirements for data are specified in more details and everybody speaks the same language, export might be automated to some degree.
Go to “Configuration:Exports” and create new data export that can be set to periodic.
Some azure function should be created to import data into data source for BI


# Cost management API

![Azure Cost Management API](\assets\azure-costs\azure cost management API.png)

If financial department is hungry for “rich daily” data [Azure Cost Management REST API](https://docs.microsoft.com/en-us/rest/api/cost-management/) can be used.
Architecture become a little more complicated, yet straight forward.
3 Azure functions connected via ServiceBus are used
1.	Scheduled function. Creates tasks for data extraction for every subscription in the account
2.	Queries data from API and sends it to Bus in single chunk
3.	Ingest data recieved from bus into storage 

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

You can play with API Query [on Microsoft documentation site](https://docs.microsoft.com/en-us/rest/api/cost-management/query/usage#exporttype)
