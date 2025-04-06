# Metric Definitions – New EBM Dashboard

This document contains all key metrics used in the dashboard, along with descriptions and assumed DAX formulas. Adjust table/column names to match your actual Power BI model.

> Note: Data is dummy and anonymized. Metrics and definitions are derived from dashboard logic for public demo use.

---

## 1. Production Defects

### Total_Defects
- *Description:* Total number of production defects logged across all teams.
DAX
Total_Defects = COUNTROWS(Defects)


### Defects_By_Developer
- *Description:* Number of defects attributed to each developer.
DAX
Defects_By_Developer = CALCULATE(COUNTROWS(Defects), ALLEXCEPT(Defects, Defects[Developer]))


### Defects_By_Month
- *Description:* Number of defects reported each month.
DAX
Defects_By_Month = CALCULATE(COUNTROWS(Defects), ALLEXCEPT(Defects, Defects[Month]))


---

## 2. Release Frequency

### Total_Releases
- *Description:* Total number of releases across all teams.
DAX
Total_Releases = COUNTROWS(Releases)


### Releases_By_Developer
- *Description:* Number of releases initiated by each developer.
DAX
Releases_By_Developer = CALCULATE(COUNTROWS(Releases), ALLEXCEPT(Releases, Releases[Developer]))


### Releases_By_Month
- *Description:* Total releases distributed by month.
DAX
Releases_By_Month = CALCULATE(COUNTROWS(Releases), ALLEXCEPT(Releases, Releases[Month]))


---

## 3. Developer Cycle Time

### Avg_Cycle_Time
- *Description:* Average time taken to complete a task or delivery per developer.
DAX
Avg_Cycle_Time = AVERAGE(CycleTime[Hours])


### Max_Cycle_Time
- *Description:* Maximum time a task has taken per developer.
DAX
Max_Cycle_Time = MAX(CycleTime[Hours])


### Tasks_Over_30Hrs
- *Description:* Number of tasks taking more than 30 hours to complete.
DAX
Tasks_Over_30Hrs = CALCULATE(COUNTROWS(CycleTime), CycleTime[Hours] > 30)


---

## 4. Developer Participation

### Developer_Team_Count
- *Description:* Number of teams a developer is assigned to.
DAX
Developer_Team_Count = DISTINCTCOUNT(TeamAssignment[Team])


### Top_Multi_Team_Developers
- *Description:* Top developers based on number of different teams they support.
DAX
TOPN(3, VALUES(Developer[Name]), [Developer_Team_Count], DESC)


---

## 5. Team Happiness

### Avg_Team_Happiness
- *Description:* Average happiness score per developer or team.
DAX
Avg_Team_Happiness = AVERAGE(Happiness[Score])


### Happiness_By_Sprint
- *Description:* Tracks changes in happiness over sprints.
DAX
Happiness_By_Sprint = AVERAGEX(VALUES(Happiness[Sprint]), CALCULATE(AVERAGE(Happiness[Score])))


---

## 6. Agile Adoption & Backlog

### Active_Teams
- *Description:* Teams currently active.
DAX
Active_Teams = CALCULATE(COUNTROWS(Teams), Teams[Status] = "Active")


### Agile_Framework_Count
- *Description:* Count of teams using Scrum vs Kanban.
DAX
Scrum_Count = CALCULATE(COUNTROWS(Teams), Teams[Framework] = "Scrum")
Kanban_Count = CALCULATE(COUNTROWS(Teams), Teams[Framework] = "Kanban")


### Product_Types
- *Description:* Project classification (Lifetime, Scoped, etc.)
DAX
Product_Type_Count = COUNTROWS(DISTINCT(Projects[ProductType]))


---

## 7. Submission Metrics

### Submission_Count
- *Description:* Number of team members who submitted feedback or entries.
DAX
Submission_Count = COUNTROWS(Happiness)


### Avg_Submission_Per_Member
DAX
Avg_Submission_Per_Member = AVERAGEX(VALUES(Happiness[Member]), CALCULATE(COUNTROWS(Happiness)))
