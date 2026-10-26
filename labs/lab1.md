# Lab 1

**Name:**Fanjun Xu
**Due:** Sunday, October 25 at 11:59 PM
**Submit:** your repo URL in the Lab 1 assignment on CourseWorks

| Part | Where | File |
|---|---|---|
| **In class:** first DuckDB query on the penguins data | Session 1 | `notebooks/lab1_first_query.ipynb` |
| **At home:** two related tables of your own in DuckDB | Before Sunday | `notebooks/lab1_home_your_data.ipynb` |
| **At home:** lifecycle map of your data | Before Sunday | This file (below) |

---

## Lifecycle map of your data

In class we named five stages every data system has. Trace them for the two tables you loaded at home. Some rows ask what you did this week; the last two ask you to look ahead to the system you'll build by December. Educated guesses are fine; say where you're unsure.

**My tables:** (Queens_HPD_Complaints_Problems_2025) + (Queens_HPD_Violations_2025)


| Stage | For your data | Your answer |
|---|---|---|
| **1. Ingestion** | Where did each file come from, and how did you get it (portal download, export, API)? If the source changed its format tomorrow, what would break? | Both datasets came from NYC Open Data and were published by HPD. I downloaded them as CSV files. If the column names or formats changed, some of my SQL queries might stop working. |
| **2. Validation** | What did `DESCRIBE`, `SUMMARIZE` and the key check reveal? Name one check you'd want to run *every time* new data arrives. | DuckDB identified the main data types correctly. There were no missing building IDs or duplicate problem IDs in the complaints table, and 6,642 building IDs appeared in both tables. I would check missing IDs and duplicates whenever new data arrives. |
| **3. Transformation** | What did you change to make the data usable (e.g. the missing-value fix)? What would you still need to clean, join or calculate to answer your question? | I loaded the CSV files into DuckDB and checked the missing-value settings. The building ID columns have different names, so I would match them when joining the tables. I would also group records by building to avoid double-counting. |
| **4. Scheduling** | How often does the source publish updates (check its website)? If your app used this data, how often would it need to refresh to stay useful, and why? | The HPD datasets are updated regularly on NYC Open Data. I would refresh the data weekly for a housing information tool so users can access recent complaints and violations. |
| **5. Interface** | Who would want answers from this data? Write the plain-English question they'd ask, and what a good answer would show them. | Renters could use this data to check housing conditions before choosing an apartment. They might ask, "What maintenance problems have been reported in this building?" A useful answer would show the types, numbers, and dates of complaints and violations. |


**Which stage feels most fragile for your data, and why?** (two or three sentences)
I think validation is the most fragile stage because the two datasets do not always cover the same buildings. Also, one building can have multiple complaints and violations, so joining the tables without checking could lead to double-counting.
---

## Checklist before you submit

- [x] In-class notebook: every cell ran, including your own query and three observations
- [x] At-home notebook: both CSVs in `data/`, both tables loaded and checked, the key relationship described, five queries each with a one-line note, three observations, two dataset cards
- [x] This lifecycle map filled in, plus the "most fragile" answer
- [x] Every notebook saved (Cmd+S), then committed and pushed
- [x] Repo URL pasted into CourseWorks
