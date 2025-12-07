# Airline Loyalty Program Analysis

## 1. Project Overview
**Objective:**
To analyze the impact of the marketing campaign on the growth of the loyalty program. Specifically, I aim to determine **whether the campaign successfully increased the number of new memberships** by analyzing historical enrollment data.

**Data Source & Citation:**
* **Dataset:** Airline Loyalty Program
* **Source:** [Maven Analytics Data Playground](https://www.mavenanalytics.io/data-playground)
* **License/Attribution:** This dataset is provided by Maven Analytics for educational and portfolio usage.

**Tools Used:**
* **SQL (Google BigQuery):** Data cleaning, time-series aggregation.
* **Tableau:** Data visualization.

---

## 2. Process (Data Cleaning & Validation)
Before analyzing the campaign's impact, I performed a data audit using SQL in BigQuery to ensure the reliability of the dataset.

### Data Audit Steps:
I verified that there were no missing values (NULLs) in the critical columns required for this analysis: `Loyalty Number` (to count members) and `Salary` (to evaluate member quality).

### SQL Queries & Results:

**1. Checking for NULLs in Loyalty Number**
```sql
SELECT
    COUNT(*) AS null_loyalty_number_count
FROM
    `airline_data.flight_activity`
WHERE
    `Loyalty Number` IS NULL;
