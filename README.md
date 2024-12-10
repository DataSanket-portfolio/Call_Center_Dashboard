
# Pizza Sales Dashboard

## Problem Statement

This project aimed to enhance transparency and provide actionable insights for a telecom company's call center operations. By analyzing call data, the company can monitor agent performance, improve customer satisfaction, and identify long-term trends in customer and agent behavior.

### Steps followed 

- Data Extraction with SQL:

    (a) Queried the database to extract key metrics and visualization data.

    (b) Generated data outputs for KPIs such as overall customer satisfaction, total calls answered/abandoned, average speed of answer, and agent performance.

    (c) Wrote SQL queries to create datasets for visualizing trends like calls by time and agent behavior.

    (D) Documented all queries and their corresponding results in a Word file with screenshots of the outputs for reference.



- Data Preparation and Cleaning:

    (a)Ensured data integrity by validating columns such as call ID, agent name, date, and time.

    (b)Handled missing or inconsistent values in the dataset.

- Dashboard Development in Power BI:

    (a) Imported the prepared data into Power BI Desktop.

    (b) Designed visuals to represent KPIs and trends effectively, including:

    - Donut Charts: 
        - Answered vs. unanswered calls.
        - Resolved vs. unresolved queries.
        - Customer satisfaction categories: Highly satisfied, Satisfied, Neutral, and Not Satisfied.

    - Stacked Bar Graph:
        - Customer satisfaction levels segmented by agent.

    - Column Chart:
        - Average handling time for each agent.

- ### Slicers:

Filters for Agent Names, Months (data covers three months), and Query Topics (e.g., Admin Support, Contract Related, Payment Related, Streaming, Technical Support).

        
# SQL queries
### KPIs

    (1) Overall customer rating – Average of all customer rating

select AVG(Satisfaction_rating) as Overall_Customer_Rating from call_center

![image](https://github.com/user-attachments/assets/f45b2df3-95c4-4a19-adb6-526862a1f873)


    (2) Total Calls Answered

select count(Answered_Y_N) as Total_Calls_Answered from call_center
where Answered_Y_N = 1
select cast(sum(total_price)/ count(distinct order_id) as decimal(10,2)) as Avg_Order_Value from pizza_sales

![image](https://github.com/user-attachments/assets/9f712ded-2a6d-427a-89fe-06b011f505de)


    (3) Total Calls Abandoned

select count(Answered_Y_N) as Total_Calls_Abandoned from call_center
where Answered_Y_N = 0
select SUM(quantity) as Total_Pizzas_Sold from pizza_sales

![image](https://github.com/user-attachments/assets/d0231f79-59db-4148-b864-e4121b3114e8)


    (4)Avg Talk Duration by each agent (Seconds/ Time)

select Agent,
Convert(varchar,Avg(DATEDIFF(second,'00:00:00',AvgTalkDuration)),108)
as Avg_Talk_in_seconds from call_center
group by Agent
order by Avg_Talk_in_seconds DESC

![image](https://github.com/user-attachments/assets/b7b44fa9-a483-4bec-a386-a116c2c54f37)

select Agent,
Convert(varchar,DATEADD(second,Avg(DATEDIFF(second,'00:00:00',AvgTalkDuration)), '00:00:00'),108)
as Avg_Talk_Duration from call_center
group by Agent
order by Avg_Talk_Duration DESC

![image](https://github.com/user-attachments/assets/aaef4f6c-ed60-4314-95c9-d2d720b15133)



    (5)Total Calls Answered and Not Attended by each Agent

 - Answered
select Agent, count(Answered_Y_N) as Total_Calls_Ans from call_center where Answered_Y_N = 1
group by Agent order by Total_Calls_Ans DESC

![image](https://github.com/user-attachments/assets/75f7cba3-2de7-4dae-8815-4a242a92f84b)

- Unanswered

select Agent, count(Answered_Y_N) as Total_Calls_Unanswered from call_center where Answered_Y_N = 0
group by Agent
order by Total_Calls_Unanswered DESC

![image](https://github.com/user-attachments/assets/d2b4bce0-dd6d-407a-8002-cfe468cf9353)

    (6) Calls by Time

select Agent,
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) <=60 then 1 else 0 End) as "<= 1 minute",
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) > 60
And(datediff(second,'00:00:00',AvgTalkDuration)) <=120 then 1 else 0 End) as "1-2 minutes",
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) > 120
And(datediff(second,'00:00:00',AvgTalkDuration)) <=180 then 1 else 0 End) as "2-3 minutes",
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) > 180
And(datediff(second,'00:00:00',AvgTalkDuration)) <=240 then 1 else 0 End) as "3-4 minutes",
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) > 240
And(datediff(second,'00:00:00',AvgTalkDuration)) <=300 then 1 else 0 End) as "4-5 minutes",
sum(case when(datediff(second,'00:00:00',AvgTalkDuration)) > 300
And(datediff(second,'00:00:00',AvgTalkDuration)) <=360 then 1 else 0 End) as "5-6 minutes",
Sum(case when(datediff(second,'00:00:00', AvgTalkDuration)) >360 then 1 else 0 End) as ">=6 minutes"
from call_center
group by Agent;

![image](https://github.com/user-attachments/assets/dcb6c4e1-e51f-4ac2-ad34-07653dae9662)


    (7) Avg Speed of answer

select Agent, AVG(Speed_of_answer_in_seconds)
as Avg_Answering_Speed from call_center
group by Agent
order by Avg_Answering_Speed DESC
select datename(Month, order_date) as Month, count(distinct order_id) as Total_Orders from pizza_sales
group by datename(Month, order_date)
order by Total_Orders DESC

![image](https://github.com/user-attachments/assets/93c6fadf-c8c2-41af-a5f8-2e8305323261)


    (8) Agent’s performace Quandrant – Avg Handling time vs Calls Answered

Select Agent, Avg(Datediff(second,'00:00:00',AvgTalkDuration)) as Avg_Handling_Time,
count(Answered_Y_N) as Calls_Answered from call_center where Answered_Y_N = 1
Group by Agent

![image](https://github.com/user-attachments/assets/8e82f7f8-aba2-497e-bcea-d8acb7564e5c)


# Snapshot of Dashboard (Power BI) - Call Center
![image](https://github.com/user-attachments/assets/13211117-d6c1-40eb-96bd-736ea25fe05f)


# Insights

- Customer Satisfaction:
    Visualized satisfaction levels to identify areas of improvement in customer interactions.

- Call Handling Efficiency: Identified agents with high average handling times or low call resolution rates for targeted coaching.

- Call Trends: Monitored call volumes by time and query topics to optimize staffing schedules.

- Agent Performance: Created an agent performance quadrant comparing average handling time vs. calls answered.

# Key Visuals
- KPIs:
    - Average Answering Speed.
    - Total Inbound Calls.

- Charts:
    - Donut charts for answered vs. unanswered calls and resolved vs. unresolved queries.
    - Stacked bar graph for customer satisfaction by agent.
    - Donut chart for customer satisfaction levels (Highly Satisfied, Satisfied, Neutral, Not Satisfied).
    - Column chart showing average handling time for each agent.

- Filters:
    - Slicers for agent names, months, and query topics for detailed data exploration.

# Tools Used
SQL: For data extraction and KPI calculation.

Power BI: For data visualization and dashboard creation.

DAX: For creating custom measures and enhancing interactivity.
