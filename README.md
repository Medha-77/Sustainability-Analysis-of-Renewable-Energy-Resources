# Project: Renewable Energy Resource Analysis

## Overview

This project, **Renewable Energy Resource Analysis**, is part of my Data Analyst portfolio and demonstrates my ability to leverage cloud technologies and data visualization tools to analyze complex datasets and extract actionable insights within the renewable energy sector.

**Project Goal:**  To assess the availability and potential of renewable energy resources, focusing on factors such as energy source distribution, regional variations, and consumption patterns.  The aim was to provide data-driven insights that could inform energy planning and resource utilization strategies.

**Key Technologies Used:**

*   **Data Warehousing:** Snowflake -  Used as a cloud data warehouse for efficient storage, management, and querying of large renewable energy datasets.
*   **Data Integration & Cloud Storage:** AWS (likely S3) - Leveraged AWS cloud services, particularly S3, for accessing, managing, and storing the agricultural datasets and for secure data integration with Snowflake.
*   **Data Visualization & Dashboarding:** Tableau - Utilized Tableau Desktop to connect to Snowflake and develop interactive dashboards for visualizing and exploring renewable energy resource data.

![Image](https://github.com/user-attachments/assets/75cb98ba-db9e-4b58-9024-bb2ee940c3e6)


![Image](https://github.com/user-attachments/assets/125d86ec-9c6b-4797-8e43-e9a60801bf67)


![Image](https://github.com/user-attachments/assets/8ed276cf-ca96-4f87-a18e-4fe888c23364)


![Image](https://github.com/user-attachments/assets/867eaa30-f258-4ee8-a2c3-ff86a945a31a)


![Image](https://github.com/user-attachments/assets/88a185b4-b363-42e6-bab9-abd00e5ec926)


![Image](https://github.com/user-attachments/assets/21523090-0a16-40e2-9aad-22b17184d985)


![Image](https://github.com/user-attachments/assets/1f1d24fc-4a66-498f-a9e7-6aa368927587)



*   **Data Transformation & Preparation:** Snowflake SQL - Employed Snowflake SQL for extensive data transformation, cleaning, and preparation tasks to ensure data quality and analytical readiness.


## Snowflake SQL Code for Data Management and Transformation

The following Snowflake SQL code was a crucial part of this project, demonstrating my skills in:

*   **Secure Data Integration:**  Setting up secure connections between AWS S3 and Snowflake using Storage Integrations and IAM Roles.
*   **Data Warehousing Architecture:**  Creating databases, schemas, and tables within Snowflake to structure and organize the renewable energy dataset.
*   **ETL Processes:**  Loading data from AWS S3 into Snowflake using the `COPY INTO` command.
*   **Data Cleaning and Transformation:**  Writing and executing SQL scripts to cleanse, standardize, and transform the data for effective analysis.

```sql
CREATE OR REPLACE STORAGE INTEGRATION Tableau_Integration
  TYPE = EXTERNAL_STAGE
  STORAGE_PROVIDER = 'S3'
  ENABLED = TRUE
  STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::881490122441:role/Tableau.role'
  STORAGE_ALLOWED_LOCATIONS = ('s3://Tableau.project07/')
  COMMENT = 'Optional Comment';


  --description Integration Object
  desc integration Tableau_Integration;



CREATE database tableau;

create schema tableau_Data;

create table tableau_dataset (
Household_ID	string,Region	string,Country	string,Energy_Source	string,
Monthly_Usage_kWh	float,Year	int,Household_Size	int,Income_Level	string,
Urban_Rural	string,Adoption_Year	int,Subsidy_Received	string,Cost_Savings_USD float




);
DESC INTEGRATION tableau_Integration;

select * from TABLEAU_DATASET;


ALTER STORAGE INTEGRATION tableau_Integration
SET STORAGE_ALLOWED_LOCATIONS = ('s3://tableau.project07');
DESC STORAGE INTEGRATION tableau_Integration;

CREATE STAGE tableau.tableau_Data.tableau_stage
URL = 's3://tableau.project07'
STORAGE_INTEGRATION = tableau_Integration;
//desc stage s1

//drop stage s1;


copy into tableau_dataset
from @tableau_stage
file_format = (type=csv field_delimiter=',' skip_header=1 )
on_error = 'continue';

LIST @tableau_stage;

select * from tableau_dataset;
SELECT region,count(*) from tableau_dataset group by region;

create table energy_consumption as select *from tableau_dataset;

select * from energy_consumption;
select income_level, count (*) from energy_consumption group by income_level;


--Increase MONTHLY_USAGE_KWH
-- Update 1
SELECT DISTINCT income_level FROM energy_consumption;
UPDATE energy_consumption SET "MONTHLY_USAGE_KWH" = "MONTHLY_USAGE_KWH" * 1.1 WHERE LOWER(income_level) = 'low';

-- Update 2
UPDATE energy_consumption SET monthly_usage_kwh = monthly_usage_kwh * 1.2 WHERE LOWER(TRIM(income_level)) = 'middle';

-- Update 3
SELECT DISTINCT income_level FROM energy_consumption;
UPDATE energy_consumption SET "MONTHLY_USAGE_KWH" = "MONTHLY_USAGE_KWH" * 1.3 WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';

--Increase COST_SAVINGS_USD
-- Update 1: Increase COST_SAVINGS_USD by 10% for 'low' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.1
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'low';

-- Update 2: Increase COST_SAVINGS_USD by 20% for 'middle' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.2
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'middle';

-- Update 3: Increase COST_SAVINGS_USD by 30% for 'high' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.3
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';


  --description Integration Object
  desc integration Tableau_Integration;



CREATE database tableau;

create schema tableau_Data;

create table tableau_dataset (
Household_ID	string,Region	string,Country	string,Energy_Source	string,
Monthly_Usage_kWh	float,Year	int,Household_Size	int,Income_Level	string,
Urban_Rural	string,Adoption_Year	int,Subsidy_Received	string,Cost_Savings_USD float




);
DESC INTEGRATION tableau_Integration;

select * from TABLEAU_DATASET;


ALTER STORAGE INTEGRATION tableau_Integration
SET STORAGE_ALLOWED_LOCATIONS = ('s3://tableau.project07');
DESC STORAGE INTEGRATION tableau_Integration;

CREATE STAGE tableau.tableau_Data.tableau_stage
URL = 's3://tableau.project07'
STORAGE_INTEGRATION = tableau_Integration;
//desc stage s1

//drop stage s1;


copy into tableau_dataset
from @tableau_stage
file_format = (type=csv field_delimiter=',' skip_header=1 )
on_error = 'continue';

LIST @tableau_stage;

select * from tableau_dataset;
SELECT region,count(*) from tableau_dataset group by region;

create table energy_consumption as select *from tableau_dataset;

select * from energy_consumption;
select income_level, count (*) from energy_consumption group by income_level;


--Increase MONTHLY_USAGE_KWH
-- Update 1
SELECT DISTINCT income_level FROM energy_consumption;
UPDATE energy_consumption SET "MONTHLY_USAGE_KWH" = "MONTHLY_USAGE_KWH" * 1.1 WHERE LOWER(income_level) = 'low';

-- Update 2
UPDATE energy_consumption SET monthly_usage_kwh = monthly_usage_kwh * 1.2 WHERE LOWER(TRIM(income_level)) = 'middle';

-- Update 3
SELECT DISTINCT income_level FROM energy_consumption;
UPDATE energy_consumption SET "MONTHLY_USAGE_KWH" = "MONTHLY_USAGE_KWH" * 1.3 WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';

--Increase COST_SAVINGS_USD
-- Update 1: Increase COST_SAVINGS_USD by 10% for 'low' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.1
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'low';

-- Update 2: Increase COST_SAVINGS_USD by 20% for 'middle' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.2
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'middle';

-- Update 3: Increase COST_SAVINGS_USD by 30% for 'high' income level
UPDATE energy_consumption
SET "COST_SAVINGS_USD" = "COST_SAVINGS_USD" * 1.3
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';

