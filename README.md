# WORKFLOW-ORCHESTRATION-KESTRA
## Project Overview 
This project focuses on leveraging Kestra as a modern workflow orchestartion tool to simpify the building of data pipelines to ingest and schedule workflows of our data tasks into our GCP bucket and Big Query Dataset to provide insights into a series of questions belwo. To provide more context on Kestra , it is an open-source event based workflow orchestration tool designed for data workflows and automation. It allows you to define workflows using YAML and supports various integrations like databases (PostgreSQL), cloud storage (GCP, AWS), and transformation tools (dbt).

This README file provides guidelines for using Kestra to move data into our GCP Bucket (Datalake) and create datasets in BIG QUERY as well as show case the  needed queries to solve the required questions.


## Answers to Kestra and GCP/BIG QUERY Questions : 
### Question 1. Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?
![HW2 Q1](https://github.com/user-attachments/assets/d2c2e200-278b-4d65-bd2c-9620b43a71ba)
ANSWER: 128.3 MB

### Question 2. What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?
Since the file variable is defined as :
```yaml
file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv"
```

This means that the variable will be dynamically replaced based by my inputs upon execution of the flow (taxi, year, month).
![H2 Q2B](https://github.com/user-attachments/assets/62d0c710-d522-42ad-b838-ce0bf5e4bc06)
![H2 Q2](https://github.com/user-attachments/assets/704dcb29-6506-4b5c-80b9-8fcc60f8fe18)

ANSWER: green_tripdata_2020-04.csv

### Question 3. How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?
![HW2 Q3](https://github.com/user-attachments/assets/ba4c581e-d848-434f-ad50-2828e043121c)

``` SQL
SELECT COUNT(*) FROM  `decisive-depth-449700-j5.dezoomcamp.yellow_tripdata`
 WHERE 
 filename LIKE 'yellow_tripdata_2020%'
```
ANSWER: 24,648,499

### Question 4. How many rows are there for the Green Taxi data for all CSV files in the year 2020?

![HW2 Q4](https://github.com/user-attachments/assets/8db31ffc-2da0-4707-9b50-7e9592a1ca02)

``` SQL
SELECT COUNT(*) FROM `decisive-depth-449700-j5.dezoomcamp.green_tripdata`
 WHERE 
 filename LIKE 'green_tripdata_2020%'
```
ANSWER: 1,734,051


### Question 5. How many rows are there for the Yellow Taxi data for the March 2021 CSV file?

![HW2 Q5](https://github.com/user-attachments/assets/e9ab3e32-d485-4911-a2a4-abf9f6f7eef6)

``` SQL
SELECT COUNT(*) FROM  `decisive-depth-449700-j5.dezoomcamp.yellow_tripdata`
 WHERE 
 filename LIKE 'yellow_tripdata_2021-03%'
```
ANSWER: 1,925,152

### Question 6. How would you configure the timezone to New York in a Schedule trigger?
To set the timezone to New York in OUR Kestra Schedule trigger, we must add a timezone property with the "America/New_York" value, We would need to adjust the trigger task in our  yaml schedule flow by inlcuding timezone as below
``` yaml
triggers:
  - id: scheduled_trigger
    type: io.kestra.core.models.triggers.types.Schedule
    cron: "0 9 * * *"  # Runs daily at 9 AM
    timezone: "America/New_York"
```

ANSWER: Add a timezone property set to America/New_York in the Schedule trigger configuration
