# Introduction to Databricks
## What is Databricks?
Databricks functions as a cloud-based data warehouse, akin to a smart storage system. Its standout feature is SQL compatibility, allowing users to easily query and analyze data using familiar database language. With Databricks, managing and interacting with data becomes straightforward, and accessible.

## Introduction to schemas and tables
### Schemas
Go to 'SQL Editor', open 'hive_metastore', you will see all the schemas we are using. \
The schema used in the production is 'analytics', which is the final version to present to the end user.\
### Tables
Under schema 'analytics', you will see different tables. You may look for the data you want by type in different keywords in the top search bar. For example, type in
```
customer_service
```
it will retrive all the customer service related tables.

## Essential SQL query for data extraction and process.
#### 1. Use 'and' to define all the conditions that are TRUE. For example, the date range.
```
select * 
from `hive_metastore`.`analytics`.`pbi_customer_service`
where start_date > '2023-12-31' and start_date < '2024-02-01'
order by start_date desc
```
can also written in 
```
select * 
from `hive_metastore`.`analytics`.`pbi_customer_service`
where start_date >= '2024-01-01' and start_date <= '2024-01-31'
order by start_date desc
```
#### 2. Use 'or' to define any of the condition is TRUE. For example, pull data for two teams.
```
select * 
from `hive_metastore`.`analytics`.`pbi_customer_service`
where team = 'Customer Service' or team = 'Tech Support'
```
#### 3. Use count() and distinct to see number of different columns.
```
select count(distinct category)
from `hive_metastore`.`analytics`.`pbi_customer_service`
where data_source = 'ServiceCloud'
```
#### 4. Use 'group by' to group the data.
##### Imgaine 'group by' as the 'pivot table' in Excel.
```
select data_source, team,category, count(*)
from `hive_metastore`.`analytics`.`pbi_customer_service`
where data_source = 'ServiceCloud'
group by data_source,team,category
```
#### 5. Use join to join multi tables
```
select ticket.interaction_id, ticket.agent_id, agent.position, agent.superior
from `hive_metastore`.`analytics`.`pbi_customer_service` as ticket
left join `hive_metastore`.`analytics`.pbi_customer_service_agent as agent on agent.agent_id = ticket.agent_id
```

## How to make Databricks connected to data visualization tool?
### Data connectivity to Power BI dashboard
Go to 'Partner Connect' in the bottom of the left panel, scroll down to the section 'BI and Visualization', you will see a Power BI logo there, click and download the connector. If you cannnot find Power BI directly, you can go to 'search by partner name on' on the top, type in Power BI.\
Connector can provide you all the tables in the workspace you connected, you can choose the table you want to load by opening the connector file.



