# Data Dictionary · Queens Housing Explorer

## Part 1 · About the project

**Project:** Queens Housing Explorer — a housing information tool that helps users explore recent housing maintenance problems, complaint patterns, and building-level conditions in Queens, New York.

**Source:** NYC Open Data, Housing Maintenance Code Complaints and Problems, published by the NYC Department of Housing Preservation and Development (HPD).

https://data.cityofnewyork.us/resource/ygpa-z7cr.json

**Refreshed:** Currently refreshed by running `python pipeline.py` manually. Each run retrieves up to 5,000 housing problem records from the last 14 days and rebuilds the published tables. The data is a recent sample and does not include the complete complaint history.

**Removed on purpose:** The API only retrieves records from Queens. Records without a problem ID or building ID are excluded from the clean table, and fully duplicate rows are removed.

---

## Part 2 · The tables

### Table 1: `course.main.housing_complaints`

**What it is:** The clean housing problem records table.

**One row is:** One individual housing problem reported to HPD. A complaint may include multiple problems.

**Built from:** `raw`

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| problem_id | text | Unique ID for an individual housing problem | Identifier | 29420407 |
| complaint_id | text | ID for the original housing complaint | Identifier; may repeat | 15106363 |
| building_id | text | HPD ID for the building | Identifier; may repeat | 432610 |
| received_date | timestamp | When the problem was reported | Date and time | 2026-11-15 00:07:45 |
| received_day | date | Day when the problem was reported | YYYY-MM-DD | 2026-11-15 |
| borough | text | Borough of the building | QUEENS | QUEENS |
| post_code | text | ZIP code of the building | ZIP code; may be missing | 11101 |
| major_category | text | General type of housing problem | HEAT/HOT WATER, PLUMBING, etc. | HEAT/HOT WATER |
| problem_code | text | Specific housing problem reported | Category | NO HEAT AND NO HOT WATER |
| complaint_status | text | Recorded status of the complaint | OPEN, CLOSE, etc. | OPEN |
| latitude | number | Latitude of the building | Decimal degrees | 40.748466 |
| longitude | number | Longitude of the building | Decimal degrees | -73.949282 |
| nta | text | Neighborhood Tabulation Area | Neighborhood name | Long Island City-Hunters Point |

### Table 2: `course.main.housing_daily_trend`

**What it is:** A daily trend of housing problems by category.

**One row is:** One housing problem category on one day.

**Built from:** `housing_complaints`

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| received_day | date | Date when problems were reported | YYYY-MM-DD | 2026-11-15 |
| major_category | text | General housing problem category | HEAT/HOT WATER, PLUMBING, etc. | HEAT/HOT WATER |
| problem_count | integer | Number of housing problem records in the category that day | Count | 100 (illustrative) |

### Table 3: `course.main.housing_building_summary`

**What it is:** Building-level summary of housing maintenance problems.

**One row is:** One building with at least one housing problem record in the current dataset.

**Built from:** `housing_complaints`

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| building_id | text | HPD building identifier | Identifier | 569090 |
| problem_count | integer | Number of housing problem records associated with the building | Count | 261 |
| complaint_count | integer | Number of distinct complaints associated with the building | Count | 77 |
| problem_types | integer | Number of distinct problem codes reported for the building | Count | Varies by building |
| latest_complaint | timestamp | Most recent received date for the building in the current dataset | Date and time | Varies by building |

### Table 4: `course.main.housing_top_buildings`

**What it is:** A ranking of the 20 buildings with the highest numbers of reported housing problems.

**One row is:** One building in the top 20 ranking for the current dataset.

**Built from:** `housing_building_summary`

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| building_id | text | HPD building identifier | Identifier | 569090 |
| problem_count | integer | Total housing problem records for the building | Count | 261 |
| complaint_count | integer | Number of distinct complaints | Count | 77 |
| problem_types | integer | Number of different problem codes reported | Count | Varies by building |
| latest_complaint | timestamp | Most recent reported problem date in the current dataset | Date and time | Varies by building |

---

## Part 3 · What this data can't tell you

1. The pipeline currently collects up to 5,000 records from a rolling 14-day window. It cannot show the complete complaint history of Queens or support reliable long-term comparisons.

2. A reported housing problem does not necessarily represent a confirmed housing violation. Multiple problems may also belong to the same complaint, so the number of problems and the number of complaints are different.

3. Complaint counts are not adjusted for building size or number of residents. Buildings with more complaints may have more housing units or higher reporting rates, so these numbers cannot directly measure overall housing quality.

4. The database does not currently include rental prices, available apartments, or transportation accessibility. It cannot answer questions about housing affordability or recommend apartments based on rent and commuting preferences.

---

## Part 4 · Example questions it can answer

1. What are the most common housing maintenance problems reported in Queens? (`housing_complaints`)

2. How many heating and hot water problems were reported each day? (`housing_daily_trend`)

3. Which buildings have the highest number of reported housing problems? (`housing_building_summary`)

4. Among the top 20 buildings, which have the most different types of housing problems? (`housing_top_buildings`)

5. How many distinct complaints are associated with Building 569090? (`housing_building_summary`)