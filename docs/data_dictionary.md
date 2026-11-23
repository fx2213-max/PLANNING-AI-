# Data Dictionary: Queens Housing Maintenance

This document describes the four tables published by the Queens Housing Maintenance Data Pipeline. The data comes from NYC Open Data and can be used to explore housing complaints, daily trends, and building-level maintenance problems in Queens.

## 1. Housing Complaints

**Table name on MotherDuck:`course.main.housing_complaints`

**One row is:One housing problem reported to HPD. A single complaint may contain multiple problems.

**Source:NYC Housing Maintenance Code Complaints and Problems, published by the NYC Department of Housing Preservation and Development (HPD).

Dataset: https://data.cityofnewyork.us/d/ygpa-z7cr

API: https://data.cityofnewyork.us/resource/ygpa-z7cr.json

**Refreshed:Currently refreshed manually by running `python pipeline.py`. The pipeline requests records from the last 14 days, with a maximum of 5,000 records per run.

**Removed on purpose:Records without a problem ID or building ID are excluded. Exact duplicate rows are removed, and the source is filtered to Queens.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| problem_id | VARCHAR | Unique identifier for a housing problem | ID | 29384179 |
| complaint_id | VARCHAR | Identifier for the original complaint | ID | 15086488 |
| building_id | VARCHAR | HPD identifier for the building | ID | 474682 |
| received_date | TIMESTAMP | When the housing problem was reported | Date and time | 2026-11-09 00:00:37 |
| received_day | DATE | Date of the report | YYYY-MM-DD | 2026-11-09 |
| borough | VARCHAR | NYC borough | QUEENS | QUEENS |
| post_code | VARCHAR | ZIP code of the building | ZIP code | 11372 |
| major_category | VARCHAR | General type of housing problem | Category | HEAT/HOT WATER |
| problem_code | VARCHAR | Specific problem reported | Category | NO HEAT |
| complaint_status | VARCHAR | Current status of the complaint in the retrieved record | OPEN, CLOSE, etc. | CLOSE |
| latitude | DOUBLE | Latitude of the building | Decimal degrees | 40.75 |
| longitude | DOUBLE | Longitude of the building | Decimal degrees | -73.89 |
| nta | VARCHAR | Neighborhood Tabulation Area | Neighborhood name | Jackson Heights |

### What this data can't tell you

The current pipeline only collects up to 5,000 recent records, so it does not represent all historical housing complaints in Queens.
A reported problem does not always mean that a housing violation has been confirmed.
Complaint counts can be affected by building size and residents' willingness to report problems.

### Example questions it can answer

1. What are the most common housing problems reported in Queens?
2. How many heating and hot water problems were reported recently?
3. Which neighborhoods have reported the most housing problems in the current dataset?

---

## 2. Housing Daily Trend

**Table name on MotherDuck:`course.main.housing_daily_trend`

**One row is:The number of reported housing problems in one category on one day.

**Source:Derived from `housing_complaints`, using the NYC HPD Complaints and Problems API.

**Refreshed:Rebuilt whenever the pipeline runs successfully.

**Removed on purpose:Individual complaint IDs, building information, and location details are not included because this table summarizes the data by date and problem category.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| received_day | DATE | Date when problems were reported | YYYY-MM-DD | 2026-11-16 |
| major_category | VARCHAR | Type of housing problem | Category | HEAT/HOT WATER |
| problem_count | BIGINT | Number of reported problems in that category on that day | Count | 100 |

### What this data can't tell you

1. It cannot identify individual buildings or complaints.
2. It only reflects the dates and records included in the current pipeline run.
3. Daily differences may be influenced by reporting behavior and do not necessarily reflect changes in underlying housing conditions.

### Example questions it can answer

1. How many housing problems were reported each day?
2. Which types of housing problems are most common?
3. How do heating complaints change across the dates covered by the dataset?

---

## 3. Housing Building Summary

**Table name on MotherDuck:`course.main.housing_building_summary`

**One row is:A summary of reported housing problems for one building.

**Source:Aggregated from `housing_complaints`, originally collected through the NYC HPD API.

**Refreshed:Rebuilt whenever the pipeline runs successfully.

**Removed on purpose:Individual problem records and detailed descriptions are aggregated into building-level statistics. This table keeps building IDs and summary indicators.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| building_id | VARCHAR | HPD identifier for the building | ID | 569090 |
| problem_count | BIGINT | Total number of housing problem records for the building | Count | 261 |
| complaint_count | BIGINT | Number of distinct complaints associated with the building | Count | 77 |
| problem_types | BIGINT | Number of distinct problem codes reported | Count | — |
| latest_complaint | TIMESTAMP | Most recent received date in the current dataset for this building | Date and time | — |

### What this data can't tell you

1. Buildings with more residents may have more complaints simply because they contain more housing units.
2. The table does not show whether each reported problem has been resolved.
3. The summary only covers the records downloaded during the current pipeline run, so it should not be interpreted as a building's complete complaint history.

### Example questions it can answer

1. Which buildings have the highest number of reported housing problems?
2. How many different types of problems were reported in a building?
3. Which buildings have the most recent housing complaints?

---

## 4. Housing Top Buildings

**Table name on MotherDuck: `course.main.housing_top_buildings`

**One row is:One of the 20 buildings with the highest number of reported housing problems in the current dataset.

**Source:Derived from `housing_building_summary`, using NYC HPD housing complaint data.

**Refreshed:Rebuilt whenever the pipeline runs successfully.

**Removed on purpose:Buildings outside the top 20 are excluded. The ranking is based on the number of reported housing problem records.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| building_id | VARCHAR | HPD identifier for the building | ID | 569090 |
| problem_count | BIGINT | Number of housing problems reported for the building | Count | 261 |
| complaint_count | BIGINT | Number of distinct complaints | Count | 77 |
| problem_types | BIGINT | Number of distinct problem codes | Count | — |
| latest_complaint | TIMESTAMP | Most recent received date in the current dataset for the building | Date and time | — |

### What this data can't tell you

1. The ranking only covers buildings included in the current sample of up to 5,000 records.
2.  A higher complaint count does not necessarily mean a building has worse housing quality.
3. The ranking does not account for building size, severity of problems, or how quickly complaints were resolved.

### Example questions it can answer

1. Which 10 buildings have the most reported housing problems?
2. Which buildings have the highest numbers of distinct complaints?
3. Among the top 20 buildings, which have the greatest variety of reported problems?
