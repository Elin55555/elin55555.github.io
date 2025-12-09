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

---

## 3. Process
In this phase, I cleaned and ensured data integrity using SQL (BigQuery). My goal was to check for null values, duplicates, and data type inconsistencies before moving to analysis.

### Data Cleaning Steps

#### 1. Checking for NULL values
First, I checked for any missing values in the primary tables. Missing data in key columns like `Loyalty Number` or `Enrollment Year` could skew the analysis.
*Note: Since column names contain spaces, I used backticks (e.g., \`Loyalty Number\`) to reference them correctly in SQL.*

**Query: Check for NULLs in Customer Loyalty History**
```sql
SELECT
  COUNT(*) AS total_rows,
  SUM(CASE WHEN `Loyalty Number` IS NULL THEN 1 ELSE 0 END) AS null_loyalty_numbers,
  SUM(CASE WHEN Country IS NULL THEN 1 ELSE 0 END) AS null_countries,
  SUM(CASE WHEN `Enrollment Year` IS NULL THEN 1 ELSE 0 END) AS null_enrollment_years
FROM
  `airline_data.loyal_history`;
```
**Query: Check for NULLs in Customer Flight Activity**
```sql
SELECT
  COUNT(*) AS total_rows,
  SUM(CASE WHEN `Loyalty Number` IS NULL THEN 1 ELSE 0 END) AS null_loyalty_numbers,
  SUM(CASE WHEN Year IS NULL THEN 1 ELSE 0 END) AS null_years,
  SUM(CASE WHEN Month IS NULL THEN 1 ELSE 0 END) AS null_months,
  SUM(CASE WHEN `Total Flights` IS NULL THEN 1 ELSE 0 END) AS null_flights
FROM
  `airline_data.flight_activity`;
```
Result: No critical NULL values were found in the primary key columns.

#### 2. Checking for Duplicates
Next, I verified that Loyalty Number is unique in the customer history table to ensure there are no duplicate customer profiles.

**Query: Check for duplicate Loyalty Numbers**
```sql
SELECT
  `Loyalty Number`,
  COUNT(*) as count
FROM
  `airline_data.loyal_history`
GROUP BY
  `Loyalty Number`
HAVING
  count > 1;
```
Result: The query returned no rows, confirming that all Loyalty Numbers are unique.

#### 3. Handling Negative Values
I ran a quick check to ensure there were no negative values in columns where it wouldn't make sense (e.g., Distance, Points Accumulated).

**Query: Check for negative values**
```sql
SELECT
  COUNT(*) AS negative_values
FROM
  `airline_data.flight_activity`
WHERE
  Distance < 0 OR `Points Accumulated` < 0;
```
Result: No negative values were found.

---

## 4. Analyze
To answer the business questions, I performed descriptive analysis using SQL. I focused on enrollment trends and cancellation rates to evaluate the effectiveness of the 2018 promotional campaign.

### Analysis 1: Membership Growth Trend (2018 Campaign Impact)
I aggregated the number of new enrollments by year and month to identify any spikes during the campaign period (2018).

**Query: Count new memberships by Year and Month**
```sql
SELECT
  `Enrollment Year`,
  `Enrollment Month`,
  COUNT(*) AS New_Memberships
FROM
  `airline_data.loyal_history`
GROUP BY
  `Enrollment Year`,
  `Enrollment Month`
ORDER BY
  `Enrollment Year`,
  `Enrollment Month`;
```
Insight: This query allows us to visualize the growth trajectory and pinpoint the exact months where the campaign had the most significant impact.

### Analysis 2: Retention Analysis (Cancellation Rate by Cohort)
A successful campaign shouldn't just bring in numbers; it should bring in valuable, long-term members. I compared the cancellation rates of members who joined in 2018 versus previous years to check for "churn" quality.

**Query: Calculate Cancellation Rate per Enrollment Year**

```sql
SELECT
  `Enrollment Year`,
  COUNT(*) AS Total_Enrolled,
  SUM(CASE WHEN `Cancellation Year` IS NOT NULL THEN 1 ELSE 0 END) AS Total_Cancelled,
  ROUND(SUM(CASE WHEN `Cancellation Year` IS NOT NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS Cancellation_Rate_Percent
FROM
  `airline_data.loyal_history`
GROUP BY
  `Enrollment Year`
ORDER BY
  `Enrollment Year`;
```
Insight: By calculating the percentage of cancellations for each enrollment year, we can assess if the 2018 cohort is retaining as well as older cohorts.

## 5. Share
I visualized the query results using **Tableau** to communicate the findings effectively to stakeholders.

### Visualization 1: Membership Growth Spike
This line chart visualizes the trend of new memberships over time.
* **Key Finding:** There was a significant spike in new enrollments during the 2018 campaign months (February - April), indicating high initial interest.

![Membership Growth Trend](https://via.placeholder.com/600x300.png?text=Insert+Tableau+Screenshot+Here+1)

### Visualization 2: Retention Rate by Cohort
This bar chart compares the cancellation rates of members who joined during the 2018 campaign versus those who joined in previous years.
* **Key Finding:** While the 2018 cohort brought in volume, the cancellation rate is higher compared to the 2017 cohort. This suggests that while the campaign was effective for acquisition, it attracted less loyal customers.

![Retention Rate Comparison](https://via.placeholder.com/600x300.png?text=Insert+Tableau+Screenshot+Here+2)


---

## 6. Act
Based on the analysis, I have derived the following conclusions and recommendations for the marketing team.

### Conclusion
1.  **Campaign Effectiveness:** The 2018 promotional campaign successfully achieved its goal of increasing new member volume. Enrollments reached an all-time high during the campaign period.
2.  **Quality of Acquisition:** However, the retention rate for these new members is lower than the historical average. The campaign attracted "promotion-seekers" who were more likely to churn after the initial offer period.

### Recommendations
1.  **Shift Focus to Retention:** For future campaigns, implement an immediate "onboarding program" to engage new members within the first 3 months to prevent early churn.
2.  **Targeted Re-engagement:** Launch a specific re-engagement campaign targeting the 2018 cohort, offering incentives for their next flight booking to boost their lifetime value.
3.  **Refine Targeting:** Analyze the demographics of the high-churn group further to refine audience targeting for the next acquisition campaign, ensuring we attract long-term travelers rather than one-time users.
