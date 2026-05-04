# Game Day Shock: How Iowa State Football Reshapes Ames
A four-year study of Iowa State home football games and their measurable effect on Ames, Iowa, using public data on wholesale liquor activity and traffic crashes.
Most home games are not the demand shock people assume. The actual signal hides inside game type — Cy-Hawk (Iowa vs. Iowa State) weeks behave nothing like regular home-game weeks, and night games carry vastly higher impaired-driver crash risk than early-kickoff games.

---

## Summary

| Finding | Number |
|---|---|
| Cy-Hawk Thursday wholesale liquor restocking vs. typical Thursday | **3.5×** ($255,830 vs. $73,748) |
| Regular home-game week wholesale liquor activity vs. control week | **Statistically indistinguishable** |
| Drug/alcohol-related crash share on night home games vs. control Saturdays | **13×** (9.5% vs. 0.7%) |
| Saturday transactions in the liquor dataset | **20 of 245,400** — the clue that reframed the whole project |

## What's in this repo
The repository is organized into notebooks for liquor and crash analysis, a deck folder with the final presentation, and this README documenting the project.

## The dataset reframing
 
The original hypothesis was *"do fans drink more on game day?"* Early in cleaning, the day-of-week distribution showed only 20 Saturday transactions across 245,400 rows. That's not "fans drink elsewhere on Saturday" - it's the dataset telling us this is exclusively **Class E wholesale activity**: B2B restocking by grocery and convenience stores, not consumer purchases. The hypothesis had to change to *"do retailers pre-stock ahead of game day?"* and the answer was different.
 
This shift is the methodological backbone of the project. Most regular home games don't change retailer behavior at all - only Cy-Hawk does.
 
## Methodology
 
1. **Filter** Iowa Liquor Sales (245k rows, 2022–2025) and Iowa Crash Data to the three Ames ZIP codes (50010, 50011, 50014).
2. **Build** a 26-game home-game schedule (2022–2025) with kickoff times, Cy-Hawk flags, conference flags, and an early-vs.-night classification at the 17:00 cutoff.
3. **Join** daily data to the schedule via `YearWeek` (`W-SUN` period), labeling each week as Control / Regular Home Game / Cy-Hawk.
4. **Compare** mean values across week types, segmented by day of week to locate where any spike actually sits.
5. **Synthesize** liquor and crash findings into recommendations differentiated by game type.
## Recommendations
 
**For Ames businesses.** Stock normally for most home games; reserve aggressive pre-positioning for Cy-Hawk only. Treating every home game as a windfall is a working-capital error.
 
**For ISU operations.** Scale DUI patrols, post-game shuttle service, and traffic resources by *game type*, not just "home game vs. away." Night games deserve materially more coverage than early kickoffs.
 
**For city services.** The 13× DUI crash spike on night games is an actionable, season-by-season planning input - every schedule release is a forecast of risk.
 
## Tools
 
Python (pandas, NumPy, matplotlib, seaborn) in Google Colab. Excel for early data inspection.
 
## Data sources
 
- [Iowa Liquor Sales](https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy) - Iowa Open Data Portal
- [Iowa Crash Data](https://data.iowa.gov/Vehicles-Transportation/Vehicle-Crashes/eyqc-dr6c) - Iowa Open Data Portal
Both datasets filtered to Ames, 2022–2025.
 
## Project context
 
This was the term project for **MIS 3116 (Data Visualization)** at Iowa State, fall 2025. Five-person team; I owned the liquor analysis end-to-end and contributed to the synthesis. The project deliverables were a 15-minute presentation deck and a written report; both are in `/deck`.
 
## Author
 
**Cameron Devereaux** — MIS '26, Iowa State University
[LinkedIn](https://www.linkedin.com/in/cameron-devereaux-847474267/) · camdev98@gmail.com
