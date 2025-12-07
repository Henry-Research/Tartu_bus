# 2.4.1_Validations_by_stops files — README

This is closed-source data which was kindly leased by the Tartu inner city bus administration. 
Primary point of contact and provided of data was Iver Olaf

DUE TO THE SIZE of the Ticket_validations data set (> 1 Gbp) it cannot be uploaded to git, instead a subset of the data can be found at /validations_by_stops_subset.csv

## Overview
This dataset contains **ticket validation records** for the Tartu inner-city bus network from **01 January 2016 to 30 November 2024**.  
Each row represents the number of passengers validating tickets at a specific stop during a specific trip.  
It is used to engineer features such as **stop popularity** and **baseline bus line coverage**  for machine learning models predicting fare evasion.

---

## Columns Description

| Column | Description |
|--------|-------------|
| `Date` | Date of the trip (DD.MM.YY) |
| `Organisation` | Bus company operating the line |
| `Line` | Bus line number |
| `Departure_ID` | Unique trip identifier |
| `Trip` | Trip description or label |
| `Start` | Scheduled start time of the trip |
| `Stop` | Name of the stop where tickets were validated |
| `Stop_ID` | Unique identifier of the stop |
| `Time` | Time when the validation occurred |
| `Validation_Count` | Number of passengers validating tickets at this stop |

---

## Purpose in Analysis
- **Feature engineering:**  
  - `Validation_Count` is aggregated to compute **stop popularity** (number of passengers per stop per time window)  
  - Determine **baseline bus line coverage** per stop (number of lines serving each stop)  
- Helps the model learn patterns of **passenger flow** and predict fare evasion likelihood.

---

