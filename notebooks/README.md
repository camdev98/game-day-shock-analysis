# Notebooks
 
Two parallel analyses, each answering one half of the project's question: *do Iowa State home football games create measurable shocks to Ames?*

| Notebook | What it analyzes | Author |
|---|---|---|
| `liquor_analysis.ipynb` | Wholesale liquor sales (Class E, B2B restocking) | Cameron Devereaux |
| `crash_analysis.ipynb` | Traffic crashes — volume, severity, drug/alcohol involvement | Teammate (included for context) |
 
Both notebooks share the same game-schedule construction and merge pattern (`YearWeek` join on a `W-SUN` period), so the comparison axes — Control / Regular Home Game / Cy-Hawk, and Early / Night kickoff — are consistent across the two analyses. This made the cross-finding synthesis at the end of the project straightforward.
 
---
## `liquor_analysis.ipynb`
 
**Question (revised):** Do Ames retailers pre-stock liquor inventory ahead of Iowa State home games?
 
**Data:** Iowa Liquor Sales — 245,400 rows, 2022–2025, filtered to Ames ZIP codes 50010, 50011, 50014.
 
**What the notebook does, in order:**
 
1. **Load and clean.** Parse `Sale (Dollars)` from string-with-dollar-signs to float, including handling for parenthesized negatives. Convert `Date` to datetime.
2. **Add temporal columns.** Day of week (named and numeric), `YearWeek` as a `W-SUN` period for joining to game weeks.
3. **Discover the dataset is B2B.** Day-of-week distribution shows 20 Saturday transactions out of 245,400 rows, with everything concentrated on Wednesdays and Thursdays. This is the pivot point of the project — the dataset is exclusively Class E wholesale activity, not consumer purchases. The hypothesis changes from "do fans drink more on game day?" to "do retailers pre-stock ahead of game day?"
4. **Build the home-game schedule.** A hand-coded DataFrame of 26 home games across 2022–2025, each tagged with kickoff time, Cy-Hawk flag, conference flag, and an early-vs.-night classification at the 17:00 cutoff.
5. **Merge.** Left-join the daily sales to the game schedule on `YearWeek`. Weeks without a home game become `Control`; the rest are `Regular Game` or `Cy-Hawk`.
6. **Verify the merge.** Spot-check known weeks (e.g., 2023 Cy-Hawk week, Sept 4–11) to confirm the labels land on the right dates.
7. **Aggregate.** Group to one row per date, then compute mean daily sales per `(DayOfWeek, WeekType)` cell. Track sample sizes per cell.
8. **Visualize.** Box plot of total daily sales (Control vs. Game Week) and a grouped bar chart of mean daily sales by day of week, broken out by Control / Regular Home Game / Cy-Hawk.
**Headline output:** Cy-Hawk Thursday wholesale restocking averages $255,830 — roughly 3.5× the typical Thursday ($73,748). Regular home-game weeks are statistically indistinguishable from control weeks. The shock is concentrated in one week per year, not every home game.
 
**Tools:** Pandas, NumPy, Matplotlib, Seaborn.
 
---

## `crash_analysis.ipynb`
 
**Question:** Do Iowa State home football games change traffic crash patterns in Ames — in volume, severity, or impaired-driver involvement?
 
**Data:** Iowa Crash Data, filtered to Ames (city number 155), 2022–2025.
 
**What the notebook does, in order:**
 
1. **Load and clean.** Convert `CRASH_DATE` to datetime, coerce numerical columns to float, tag city number for filtering.
2. **Build the same game schedule** used in the liquor analysis — same 26 games, same flags, same `W-SUN` join key.
3. **Merge** crash records to the game schedule on `YearWeek`, labeling each week as Control / Early / Night / Cy-Hawk.
4. **Aggregate by week type.** Mean crashes per day-of-week per week type; pivot for side-by-side comparison.
5. **Compute percentage change** in average crashes for game-week conditions vs. control. Saturday is the focus, but Friday and Sunday also examined.
6. **Filter for drug/alcohol involvement.** Use the `DRUGALC` column, excluding the "no involvement" code (8.0), to compute the share of Saturday crashes involving impaired drivers under each condition.
7. **Examine severity.** Mean injuries per crash and mean property damage per crash, by `(DayOfWeek, WeekType)`.
8. **Add a weather control.** Examine crash counts by weather condition to flag whether outliers are confounded.
9. **Visualize.** Heatmap of mean daily crashes, grouped bar chart of percentage change vs. control, distribution plot of crashes per Saturday.
**Headline output:** Night home games show 9.5% of Saturday crashes involving drugs or alcohol vs. 0.7% on control Saturdays — a 13× rate increase. Early-kickoff games sit between, at 3.2%. Crash *volume* rises on game days but largely scales with the population surge; the *risk profile* (impaired-driver share, severity distribution) is the part that's genuinely different.
 
**Tools:** pandas, NumPy, matplotlib, seaborn. Google Colab.
 
---

## Reproducing
 
Both notebooks were written in Google Colab and load CSVs from a mounted Google Drive. To reproduce locally:
 
1. Download the Iowa Liquor Sales and Iowa Crash Data CSVs from the Iowa Open Data Portal (links in the root README).
2. Filter each to Ames before loading (or filter inline — the notebooks do this anyway).
3. Replace the `csv_path` lines in cell 0 of each notebook with local paths.
4. Run cells in order. The schedule cell (cell 8 in liquor, cell 6 in crash) is hand-coded and does not depend on external data.
Dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`. Any recent version works.
 
---

## A note on the merge pattern
 
The `YearWeek` join with `W-SUN` periods is the small piece of code worth understanding. A football week stretches Monday → Sunday, so weeks ending Sunday capture the full lead-up to a Saturday game (Wednesday/Thursday restocking, Friday travel, Saturday crashes) in a single key. The same join logic powers both notebooks, which is why the week-type labels are consistent across analyses and the cross-finding synthesis works cleanly.
