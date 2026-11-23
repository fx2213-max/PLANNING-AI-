# Findings Report: Queens Housing Maintenance

These findings are based on the 5,000 housing problem records collected through the NYC HPD API during the pipeline run.

## Finding 1

**What I found:**

Heating and hot water problems were the most common type of housing issue in the dataset, with 2,509 records out of 5,000, or about 50.2%. Unsanitary conditions came second, with 626 records.

**From table:** `course.main.housing_complaints`

**The query or row that shows it:**

sql：
SELECT
    major_category,
    COUNT(*) AS problem_count
FROM course.main.housing_complaints
GROUP BY major_category
ORDER BY problem_count DESC
LIMIT 10;


**A question a user could ask the app to discover this:**

What are the most common housing problems reported in Queens?

## Finding 2

**What I found:**

Some buildings had many more reported housing problems than others. Building 569090 had the highest number in this dataset, with 261 housing problem records associated with 77 distinct complaints.

**From table:** `course.main.housing_building_summary`

**The query or row that shows it:**

sql：
SELECT
    building_id,
    problem_count,
    complaint_count
FROM course.main.housing_building_summary
ORDER BY problem_count DESC
LIMIT 10;


**A question a user could ask the app to discover this:**

Which buildings have the most reported housing problems, and how many complaints do they have?

## Finding 3

**What I found:**

The number of reported housing problems varied across the days covered by the dataset. November 13 had the highest count among the displayed dates, with 709 problems, while November 17 had 209.

**From table:** `course.main.housing_daily_trend`

**The query or row that shows it:**

sql：
SELECT
    received_day,
    SUM(problem_count) AS total_problems
FROM course.main.housing_daily_trend
GROUP BY received_day
ORDER BY total_problems DESC;


**A question a user could ask the app to discover this:**

How did the number of reported housing problems change across different days in Queens?

---

## What this data can't tell you

The dataset only includes the 5,000 records collected during this pipeline run, so it does not represent all housing complaints in Queens. The numbers also depend on how often residents report problems and how many people live in each building. Therefore, a higher complaint count does not necessarily mean worse housing quality.
