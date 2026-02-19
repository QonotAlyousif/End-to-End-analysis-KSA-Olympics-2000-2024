# Saudi Arabia Olympics Participation and Performance Analysis (2000-2024)
Full stack data analysis project (End to End) of Saudi Arabia Olympics Participation and Performance Analysis for the period (2000-2024)

## Objective 
The project focuses on strategic resource allocation: strengthening high-representation disciplines while investing in under-supported athletes and sports to ensure sustainable and balanced Olympic participation.
## Dataset 
1- [120 years of Olympic history: athletes and results](https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results?resource=download) 

2- [Participants in the Olympics & Paralympics](https://olympic.sa/team-saudi/participants/) 
## Tools 
- MySQL Workbench
- Excel
- Power BI
## Data Cleansing 
The dataset was filtered using **SQL** to include only records related to Saudi Arabia from 2000 to 2024.
The data was then structured into three relational tables:
- Events
- Athletes
- Participant

Then the data were exporting to **Excel** for further cleansing such as:  
- standardize text cells and unify inconsistencies (such as spelling variations and formatting differences).
- complete missing value using data sourced from the official Saudi Olympic & Paralympic Committee


## Data model 
<img width="1773" height="1091" alt="Screenshot 2026-02-19 212041" src="https://github.com/user-attachments/assets/1e18951a-6035-4026-8169-4518e3ac21b3" />

## Dax Measures examples 

- **Athletes per age group** 
```
Age Group = 
VAR Age = [Participation Age]
RETURN
SWITCH(
    TRUE(),
    Age <= 20, "≤20",
    Age <= 25, "21-25",
    Age <= 30, "26-30",
    Age <= 35, "31-35",
    Age <= 40, "36-40",
    "41+")
```
- **Avarage best positions per Discipline**
```
 Best Position per Event = 
CALCULATE (
    MIN ( participants[pos] ),
    FILTER (
        participants,
        NOT ISBLANK ( participants[pos] )
    ))
``` 
```
Avg Position by Discipline = 
AVERAGEX (
    VALUES ( participants[event_id] ),
    [Best Position per Event]
)
```
- **Male/Female first participaton age** 
```
Male first participation = 
CALCULATE(
    AVERAGEX(
        SUMMARIZE(
            participants,
            participants[athlete_url],
            "FirstYear", MIN(events[event_year])
        ), CALCULATE(
            MIN(participants[Participation Age])
        )
    ),
    athletes[sex] = "M"
)
```
% of Multiple vs Single Particpants 
```
Multiple Participants % = 
VAR TotalParticipants = DISTINCTCOUNT(participants[athlete_url])
VAR MultipleCount = 
    COUNTROWS(
        FILTER(
            VALUES(participants[athlete_url]),
            CALCULATE(DISTINCTCOUNT(participants[event_id])) > 1
        )
    )
RETURN
    DIVIDE(MultipleCount, TotalParticipants, 0)
```
```
Single Participant % = 
VAR TotalParticipants = DISTINCTCOUNT(participants[athlete_url])
VAR SingleParticipantsCount = 
    COUNTROWS(
        FILTER(
            VALUES(participants[athlete_url]),
            CALCULATE(DISTINCTCOUNT(participants[event_id])) = 1
        )
    )
RETURN
    DIVIDE(SingleParticipantsCount, TotalParticipants, 0)
```
## Dashboard Screenshot 
- Dashboard overview 
<img width="2034" height="1025" alt="Screenshot 2026-02-19 101155" src="https://github.com/user-attachments/assets/0484c476-8ae2-4220-a040-aa1b07c7a2a5" />
- Year filtering example 
<img width="2035" height="1013" alt="Screenshot 2026-02-19 102707" src="https://github.com/user-attachments/assets/bb50d153-42e7-4417-8163-1918e579a9f4" />

- Drill through example 
<img width="2061" height="986" alt="Screenshot 2026-02-19 101710" src="https://github.com/user-attachments/assets/84c06aa6-de91-4022-a245-2407c7904eed" />

## Insgihts 
- **Peak Participation:** The 21–25 age group represents the highest athlete participation, marking the primary competitive peak.
- **Top Medal Sport:** Equestrianism leads in total medals compared to other sports.
- **Athlete Continuity:** 27% of athletes participated in multiple Olympic events, indicating moderate retention with room for improvement.
- **Gender Gap:** Participation is still male-dominated, though female representation is gradually increasing.
- **Performance Stability:** Results fluctuate across sports over time, while Weightlifting shows relatively stable performance trends.
## Recommendations 
- **Increase Retention:** Strengthen long-term athlete development programs to boost repeat participation.
- **Early Investment:** Focus on athletes aged 18–23 to sustain peak participation levels (21–25).
- **Support Women’s Sports:** Expand initiatives that enhance female participation and representation.
- **Stabilize Performance:** Implement continuous performance planning to reduce fluctuations.
- **Leverage Strengths:** Maintain strong support for stable and high-medal sports, especially Equestrianism.
