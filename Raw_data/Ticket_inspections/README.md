# 6.2_Ticket_control_report(s) — README

This is closed-source data which was kindly leased by the Tartu inner city bus administration. 
Primary point of contact and provided of data was Iver Olaf

## Overview
This dataset contains detailed **ticket inspection records** from the Tartu inner-city bus network, spanning **01 January 2016 to 30 November 2024**.  
It is used as the **target variable** in machine learning models predicting fare evasion, recording the number of fare evaders detected per inspection event.

---

## Columns Description

| Column | Description |
|--------|-------------|
| `Stop` | Name of the bus stop where inspection occurred |
| `Stop_code` | Unique identifier for the bus stop |
| `Line` | Bus line number |
| `Inspector` | Name of the inspector performing the check |
| `Cards_checked` | Number of passengers checked during the inspection |
| `Validated` | Number of passengers with valid tickets |
| `Unvalidated` | Number of fare evaders caught |
| `Start_Dtime` | Start timestamp of the inspection event |
| `End_Dtime` | End timestamp of the inspection event |

---

## Purpose in Analysis
- **Target feature:** `Unvalidated` is the main variable the ML model aims to predict.

---