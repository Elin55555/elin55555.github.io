# Airline Loyalty Program Analysis

## 1. Project Overview
**Objective:** To evaluate the performance of the airline's loyalty program and identify high-value customer segments. The goal is to provide data-driven recommendations for optimizing marketing strategies and increasing customer lifetime value (CLV).

**Data Source:** Airline Loyalty Program Dataset (Provided by Maven Analytics)

**Tools Used:** * **SQL (Google BigQuery):** Data cleaning, joining tables, and aggregation.
* **Tableau:** Data visualization (Planned).

---

## 2. Process (Data Cleaning & Validation)
Before diving into the analysis, I conducted a data audit using SQL in BigQuery to ensure data integrity.

### Data Audit Steps:
I specifically checked for missing values (NULLs) in critical columns such as `Loyalty Number` (Primary Key) and `Salary` (Key attribute for segmentation), as these are essential for calculating CLV and demographic analysis.

### SQL Queries & Results:

**1. Checking for NULLs in Loyalty Number**
```sql
SELECT
    COUNT(*) AS null_loyalty_number_count
FROM
    `airline_data.flight_activity`
WHERE
    `Loyalty Number` IS NULL;
