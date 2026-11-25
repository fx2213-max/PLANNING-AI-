# App evaluation

<!--
Lab 5. Ask your app five questions and judge each answer like an editor would.
Use your findings (docs/findings.md) for the first three: you already know the right answer,
so you can tell whether the app got it right.
Open "How this answer was made" every time: read the SQL and the rows, not just the sentence.
-->

**App URL:** https://florasapp.streamlit.app/

**Tables the app uses:** `course.main.housing_complaints`, `course.main.housing_daily_trend`, `course.main.housing_building_summary`, `course.main.housing_top_buildings`

## The five questions

| # | Kind | Question you asked | What the app said | SQL right? (yes / partly / no) | Verdict (good / weak / wrong) | Why |
|---|---|---|---|---|---|---|
| 1 | From finding 1 | What are the most common housing problems reported in Queens? | Pending app test | To check | To check | Check whether the app correctly groups housing problems by category and reports the counts. |
| 2 | From finding 2 | Which 10 buildings have the most reported housing problems? | Pending app test | To check | To check | Check whether the app ranks buildings by problem count and distinguishes problems from unique complaints. |
| 3 | From finding 3 | How many housing problems were reported each day? | Pending app test | To check | To check | Check whether the app groups all problem categories by date and calculates daily totals correctly. |
| 4 | Harder question | Which buildings had both heating and plumbing problems, and how many problems of each type did they report? | Pending app test | To check | To check | The SQL should filter the two categories, identify buildings with both, and count each category separately. |
| 5 | Can't answer | What is the average monthly rent for apartments in the 10 buildings with the most housing problems? | Pending app test | To check | To check | The database contains housing maintenance records but no rental prices. The app should explain that the question cannot be answered with the available data. |

**Verdict guide**

- **Good:** right number, right table, the sentence matches the rows.
- **Weak:** roughly right, but it used the wrong table, missed a filter, or the sentence overstates what the rows show.
- **Wrong:** wrong number, an error, or it made up an answer to a question the data can't answer.

## One improvement, tested

Pick your weakest answer. Change ONE thing, push, wait for the app to update (about a minute), and ask the same question again.

**Question:**

**What was wrong:**

**What I changed** (tick one and describe it):

- [ ] the data dictionary (`docs/data_dictionary.md`): a clearer meaning, units or allowed values
- [ ] `extra_instructions` in `app.toml`
- [ ] a new or changed table in `project.toml`

**Before:** (what it said)

**After:** (what it says now)

## What leaves your control

Every question sends your question, your table descriptions and some result rows to Google's free Gemini tier. In 2–3 sentences: is that acceptable for your data? What would you change before using this app with non-public data?

