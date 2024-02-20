---
layout: post
title: Steampipe plugin for SAP BTP 
---

I've been recently playing around with [Steampipe](https://steampipe.io/) to query different cloud services I use. This got me thinking, it will be very cool if I could query my SAP Business Technology Platform (SAP BTP) account with Steampipe.

![Steampipe plugin for SAP BTP demo](../images/steampipe-plugin-demo.gif)

## What is Steampipe?

From the [Steampipe website](https://steampipe.io/docs), *Steampipe exposes APIs and services as a high-performance relational database, giving you the ability to write SQL-based queries to explore dynamic data.* In short, you can query, using SQL, your cloud services via Steampipe. You can write SQL queries to know the state of your cloud resources, e.g. do you want to know how many serverless functions are running in your AWS account?
  
  ```sql
  select 
    runtime,
    count(*)
  from 
    aws_lambda_function
  group by 
    runtime;
  ```

Under the hood, Steampipe leverages PostgreSQL Foreign Data Wrappers (FDW) to provide a SQL interface to external services and systems. Given that there is a Postgres database involved, you can use it to export your data or connect it with 3rd party tools available in the Postgres ecosystem.

## Steampipe plugin for SAP BTP

The cloud platform that I interact with the most is SAP BTP. Naturally, while I was getting familiar with Steampipe I thought it would be great if a plugin for SAP BTP was available. Unfortunately, there was none, but as developers, we can always go ahead and create/contribute one.

The [Steampipe documentation](https://steampipe.io/docs/develop/overview) includes everything that is required to develop a new plugin.... how to [write a plugin](https://steampipe.io/docs/develop/writing-plugins#writing-plugins), how to [define a table](https://steampipe.io/docs/develop/writing-your-first-table#writing-your-first-table), how to convert from a JSON response to Columns, etc. Using the [Zendesk example](https://github.com/turbot/steampipe-plugin-zendesk) shown on the website as a starting point, I decided to create a plugin for SAP BTP.

> You can find the source code of the Steampipe plugin for SAP BTP on GitHub ([https://github.com/ajmaradiaga/steampipe-plugin-btp](https://github.com/ajmaradiaga/steampipe-plugin-btp)). The repository includes instructions on how to run the plugin locally.

In the PoC version of the plugin, the plugin only interacts with the [Accounts service](https://api.sap.com/api/APIAccountsService/overview), part of the [SAP BTP Core Services](https://api.sap.com/package/SAPCloudPlatformCoreServices/rest). I've defined the tables below:
	- btp_accounts_global_account
	- btp_accounts_subaccounts
	- btp_accounts_directories

When querying the tables above, the plugin will call the API on our behalf and convert the data so that it can be displayed in a table format. For example, `select * from btp_trial.btp_accounts_global_account` will call the `/accounts/v1/globalaccount` method part of the Accounts service API.

Given that there is PostgreSQL in the background, we can leverage SQL capabilities to filter, and group the data.
```sql
-- Apply filter in your queries, e.g. retrieve all subaccounts in a specific region.
select guid, display_name, region 
from btp_trial.btp_accounts_subaccounts 
where region = 'ap21'
```
```txt
+--------------------------------------+------------------+--------+
| guid                                 | display_name     | region |
+--------------------------------------+------------------+--------+
| 4e923803-c32b-4cb6-b008-738892b7cd8f | subaccount_under | ap21   |
| a6285f10-2dae-4318-b2c6-2075f2fcc3f3 | subaccount_2     | ap21   |
+--------------------------------------+------------------+--------+
```
```sql
-- Count all subaccounts per region
select region, count(1) as total
from btp_trial.btp_accounts_subaccounts
group by region 
order by total desc
```
```txt
+--------+-------+
| region | total |
+--------+-------+
| ap21   | 2     |
| us10   | 1     |
+--------+-------+
```

### Where do I see a tool like this being used?
There are plenty of developers out there who are well-versed in SQL but might not have had the time to get their heads around APIs. Not everyone is familiar with APIs and all the complexity that might surround them. That said, I'm not discouraging anyone from learning how cloud service APIs work. If you don't know how APIs work, you should invest some time learning about them. That said, this is something that might help to get familiar with the data that's exposed by these APIs.

If you made it this far, you've found it interesting to use SQL to interact with these cloud services. I plan to invest more time in this plugin and expand its capabilities to include the Entitlements, Events, Provisioning, and Resource consumption services that are part of the [SAP BTP Core Services](https://api.sap.com/package/SAPCloudPlatformCoreServices/rest). If you have any suggestions, I encourage you to share them with me via [GitHub](https://github.com/ajmaradiaga/steampipe-plugin-btp/issues).