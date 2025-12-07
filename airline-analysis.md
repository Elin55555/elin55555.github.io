# Airline Loyalty Program Analysis ✈️

## Project Overview
This case study analyzes the loyalty program data of a fictional airline. The goal is to evaluate the impact of the 2018 promotional campaigns on membership growth and retention. By leveraging SQL (BigQuery) for data processing and analysis, this project aims to provide actionable insights for the marketing team to optimize future strategies.

## 1. Ask
### Business Task
The marketing team wants to understand how the recent promotional campaign affected the loyalty program's performance. The primary objective is to determine if the campaign successfully drove new enrollments and maintained member engagement, or if it led to higher churn rates.

### Key Stakeholders
* **Marketing Director:** Interested in the ROI of the campaign and its effectiveness in acquiring new members.
* **Loyalty Program Manager:** Focuses on member retention and the long-term value of the customer base.
* **Data Analyst (Me):** Responsible for analyzing the data and providing objective recommendations.

### Key Questions
1.  How did the 2018 campaign impact the number of new memberships compared to previous years?
2.  Did the campaign attract high-value customers or lead to higher cancellation rates?
3.  What are the demographic characteristics of the members acquired during the campaign?

---

## 2. Prepare
### Data Source
The data used in this analysis is provided by [Maven Analytics](https://mavenanalytics.io/data-playground/airline-loyalty-program). It consists of two main CSV files:
* `Customer Loyalty History.csv`: Contains customer demographics, enrollment dates, cancellation dates, and loyalty status.
* `Customer Flight Activity.csv`: Contains detailed records of flight activities, points accumulated, and distance traveled.

### Tools Used
* **Google BigQuery:** Used for data warehousing, cleaning, and analysis (SQL).
* **Tableau:** Used for creating interactive data visualizations and dashboards.
* **GitHub:** Used for version control and documentation.

### Data Organization & Verification (ROCCC)
I performed a quick assessment of the data using the ROCCC framework:
* **Reliable:** The dataset is a well-structured educational dataset provided by Maven Analytics.
* **Original:** While the data is synthetic for educational purposes, it simulates real-world airline data structures.
* **Comprehensive:** It includes essential fields such as location, gender, education, salary, flight distance, and loyalty points.
* **Current:** The dataset focuses on historical data (2017-2018), which is suitable for this specific case study on past campaign performance.
* **Cited:** The data is publicly available via the Maven Analytics Data Playground.

### Setup Process
1.  **Downloaded Data:** Sourced the `.csv` files from Maven Analytics.
2.  **Cloud Setup:** Created a new project in Google Cloud Platform named `Portfolio-Airline-Analysis`.
3.  **Data Ingestion:**
    * Created a dataset named `airline_loyalty` in BigQuery.
    * Uploaded `Customer Loyalty History.csv` as the table `customer_loyalty_history`.
    * Uploaded `Customer Flight Activity.csv` as the table `customer_flight_activity`.
    * Enabled "Auto-detect" for schema to automatically identify data types (String, Integer, Float, Date).

## 3. Process
In this phase, I cleaned and ensured data integrity using SQL (BigQuery). My goal was to check for null values, duplicates, and data type inconsistencies before moving to analysis.

### Data Cleaning Steps

#### 1. Checking for NULL values
First, I checked for any missing values in the primary tables. Missing data in key columns like `Loyalty Number` or `Enrollment Year` could skew the analysis.

**Query: Check for NULLs in Customer Loyalty History**
```sql
SELECT
  COUNT(*) AS total_rows,
  SUM(CASE WHEN Loyalty_Number IS NULL THEN 1 ELSE 0 END) AS null_loyalty_numbers,
  SUM(CASE WHEN Country IS NULL THEN 1 ELSE 0 END) AS null_countries,
  SUM(CASE WHEN Enrollment_Year IS NULL THEN 1 ELSE 0 END) AS null_enrollment_years
FROM
  `airline_loyalty.customer_loyalty_history`;
