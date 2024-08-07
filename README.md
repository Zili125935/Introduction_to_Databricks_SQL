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
#### 1. Use 'and' to define all the conditions that are TRUE. 
##### Example 1 - filter tickets for specific date range.
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
##### Example 2 - filter sales data for specific date range.
```
select * 
from `ilum_prod`.`analytics`.`pbi_customer_360_customer_daily_stats`
where date > '2023-12-31' and date < '2024-02-01'
```

#### 2. Use 'or' to define any of the condition is TRUE. For example, pull data for two teams.
##### Example 1 - filter customer tickets of two teams.
```
select * 
from `hive_metastore`.`analytics`.`pbi_customer_service`
where team = 'Customer Service' or team = 'Tech Support'
```
##### Example 2 - filter sales data of two purchase order types.
```
select * 
from `ilum_prod`.`analytics`.`pbi_customer_360_customer_order_items` 
where purchase_order_type = 'CTEL' or purchase_order_type = 'ESHO'
```
#### 3. Use count() and distinct to see number of different columns.
##### Example 1 - To know how many different categories ServiceCloud has.
```
select count(distinct category)
from `hive_metastore`.`analytics`.`pbi_customer_service`
where data_source = 'ServiceCloud'
```
##### Example 2 - To know how many different purchase order type we have.
```
select count(distinct purchase_order_type)
from `ilum_prod`.`analytics`.`pbi_customer_360_customer_order_items`
```

#### 4. Use 'group by' to group the data.
##### Imgaine 'group by' as the 'pivot table' in Excel.
##### Example 1 - view the number of ServiceCloud tickets for each team and category. 
```
select data_source, team,category, count(*)
from `hive_metastore`.`analytics`.`pbi_customer_service`
where data_source = 'ServiceCloud'
group by data_source,team,category
```
##### Example 2 - view order number of each purchase order type in specific date range.
```
select order_date, purchase_order_type, count(*)
from `ilum_prod`.`analytics`.`pbi_customer_360_customer_order_items`
where order_date between '2024-01-01' and '2024-01-31'
group by order_date, purchase_order_type
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



