
# 🌍 **Renewable Energy Resource Analysis** –
## 📌 **Project Overview**  
This project, **Renewable Energy Resource Analysis**, demonstrates my ability to leverage **cloud technologies (Snowflake, AWS S3) and data visualization tools (Tableau)** to analyze large-scale renewable energy datasets.  

### 🎯 **Project Goal**  
To assess the availability and potential of **renewable energy resources** by analyzing:  
- **Energy source distribution**  
- **Regional variations**  
- **Consumption patterns**  
- **Cost savings based on energy adoption**  

The insights help in **energy planning and resource optimization.**  

---  

## 🏆 **Key Insights from the Analysis**  
✅ **Renewable energy adoption is higher in urban areas**, with solar and wind energy being the dominant sources.  
✅ **Lower-income households tend to consume less energy**, but **cost savings are higher** due to subsidies.  
✅ **High-income groups benefit more from long-term savings**, as they invest more in energy-efficient systems.  
✅ **Regions with higher adoption rates have lower average monthly energy costs.**  
✅ **Government subsidies play a significant role in increasing renewable energy adoption.**  

---  

## 🛠 **Technologies Used**  

| Technology  | Purpose  |
|-------------|---------|
| **Snowflake**  | Cloud data warehouse for storing and processing renewable energy data.  |
| **AWS S3**  | Storage of raw CSV files before loading into Snowflake.  |
| **Tableau**  | Data visualization to create **interactive dashboards** and reports. |

---

## 📂 **Project Structure**  
/data/                   # [Optional] Could contain sample CSV data files

/sql/                    # Snowflake SQL scripts for data management

    create_integration.sql           # Creates Storage Integration with AWS S3
    create_database_schema_table.sql # Creates database, schema, and table
    create_stage.sql                 # Creates external stage for S3 connection
    load_data.sql                     # Loads CSV data from S3 into Snowflake
    transform_data.sql                # Cleans and transforms data
    
/tableau/                # Contains Tableau workbook (.twb or .twbx) file

README.md               # Project documentation


---

## 🔧 **Setup Instructions**  

### 🔹 **Prerequisites**  
- **AWS account** with S3 access  
- **Snowflake account**  
- **Tableau Desktop installed**  

### 🔹 **AWS S3 Configuration**  
1. Create an **S3 bucket** (e.g., tableau.project07) in a region supported by Snowflake.  
2. Upload your **renewable energy data (CSV format)** to the S3 bucket.  

---

## 📊 **Snowflake SQL Code for Data Management & Transformation**  

### **1️⃣ Create Database, Schema, and Table**  
sql
CREATE database tableau;
CREATE schema tableau_Data;
CREATE table tableau_dataset (
    Household_ID STRING,
    Region STRING,
    Country STRING,
    Energy_Source STRING,
    Monthly_Usage_kWh FLOAT,
    Year INT,
    Household_Size INT,
    Income_Level STRING,
    Urban_Rural STRING,
    Adoption_Year INT,
    Subsidy_Received STRING,
    Cost_Savings_USD FLOAT
);


### **2️⃣ Create a Secure Storage Integration with AWS S3**  
sql
CREATE OR REPLACE STORAGE INTEGRATION Tableau_Integration
    TYPE = EXTERNAL_STAGE
    STORAGE_PROVIDER = 'S3'
    ENABLED = TRUE
    STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/YOUR_IAM_ROLE'
    STORAGE_ALLOWED_LOCATIONS = ('s3://YOUR_S3_BUCKET/')
    COMMENT = 'Snowflake integration with AWS S3 for Tableau data';
DESC INTEGRATION Tableau_Integration;


### **3️⃣ Create External Stage for Data Loading**  
sql
CREATE STAGE tableau.tableau_Data.tableau_stage
    URL = 's3://YOUR_S3_BUCKET'
    STORAGE_INTEGRATION = Tableau_Integration;


### **4️⃣ Load Data from S3 into Snowflake**  
sql
COPY INTO tableau_dataset
FROM @tableau_stage
FILE_FORMAT = (TYPE=CSV FIELD_DELIMITER=',' SKIP_HEADER=1)
ON_ERROR = 'continue';


### **5️⃣ Data Transformation: Adjusting Consumption & Cost Savings**  
sql
CREATE TABLE energy_consumption AS SELECT * FROM tableau_dataset;

-- Adjust MONTHLY_USAGE_KWH based on income level
UPDATE energy_consumption 
SET MONTHLY_USAGE_KWH = MONTHLY_USAGE_KWH * 1.1 
WHERE LOWER(income_level) = 'low';

UPDATE energy_consumption 
SET MONTHLY_USAGE_KWH = MONTHLY_USAGE_KWH * 1.2 
WHERE LOWER(TRIM(income_level)) = 'middle';

UPDATE energy_consumption 
SET MONTHLY_USAGE_KWH = MONTHLY_USAGE_KWH * 1.3 
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';

-- Adjust COST_SAVINGS_USD based on income level
UPDATE energy_consumption 
SET COST_SAVINGS_USD = COST_SAVINGS_USD * 1.1 
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'low';

UPDATE energy_consumption 
SET COST_SAVINGS_USD = COST_SAVINGS_USD * 1.2 
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'middle';

UPDATE energy_consumption 
SET COST_SAVINGS_USD = COST_SAVINGS_USD * 1.3 
WHERE LOWER(TRIM(COALESCE(income_level, ''))) = 'high';


---

## 📈 **Tableau Setup & Visualization**  
1. Open **Tableau Desktop**.  
2. Connect to **Snowflake** using the Snowflake connector.  
3. Enter:  
   - **Snowflake account identifier, username, password**  
   - **Database: tableau**  
   - **Schema: tableau_data**  
   - **Warehouse: (your warehouse name)**  
4. Use energy_consumption table to build **visualizations & dashboards.**  

---

## 📊 **Data Visualizations & Dashboards**  

💡 **Tableau Insights**:  
📌 Interactive dashboards created for **energy usage trends, cost savings, and adoption patterns.**  

### **Sample Dashboard Screenshots:**  
![Image](https://github.com/user-attachments/assets/75cb98ba-db9e-4b58-9024-bb2ee940c3e6)
![Image](https://github.com/user-attachments/assets/125d86ec-9c6b-4797-8e43-e9a60801bf67)
![Image](https://github.com/user-attachments/assets/8ed276cf-ca96-4f87-a18e-4fe888c23364)
![Image](https://github.com/user-attachments/assets/867eaa30-f258-4ee8-a2c3-ff86a945a31a)
![Image](https://github.com/user-attachments/assets/88a185b4-b363-42e6-bab9-abd00e5ec926)
![Image](https://github.com/user-attachments/assets/21523090-0a16-40e2-9aad-22b17184d985)
![Image](https://github.com/user-attachments/assets/1f1d24fc-4a66-498f-a9e7-6aa368927587)

## 📥 **How to Use the Project**  

1. Open https://github.com/Medha-77/Sustainability-Analysis-of-Renewable-Energy-Resources/blob/main/ENERGY%20CONSUMPTION.twb in Tableau.  
2. Explore **interactive dashboards & insights!**  

---

## 🎯 **Key Skills Demonstrated in This Project**  
✅ **Data Warehousing (Snowflake)** – Managing large datasets in a structured format.  
✅ **Cloud Storage (AWS S3)** – Secure data storage and integration.  
✅ **ETL Process** – Data extraction, transformation, and loading.  
✅ **SQL Data Transformation** – Cleaning, standardizing, and optimizing data.  
✅ **Data Visualization (Tableau)** – Creating insightful dashboards and reports.  

---

## 🔗 **Live Project & Repository**  
📌 **GitHub Repo:** [Sustainability Analysis of Renewable Energy Resources](https://github.com/Medha-77/Sustainability-Analysis-of-Renewable-Energy-Resources)  


