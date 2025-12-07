# GTFS Data — README

This open-souce data was retreived from : https://peatus.ee/gtfs/ 

## Overview
This folder contains GTFS (General Transit Feed Specification) data describing the Tartu bus network. GTFS provides a standard format for public transit schedules, routes, stops, and related information. It is used in this project to extract features such as bus stop locations, route density, and schedule patterns for predicting fare evasion.

All files are .csv format and can be read in with "pd.read_csv(file_path)"

---

## File Descriptions

| File | Description |
|------|-------------|
| `agency.txt` | Information about the transit agency (name, contact, timezone). |
| `calendar.txt` | Regular service schedules with start/end dates and active weekdays. |
| `calendar_dates.txt` | Exceptions to the regular service calendar (holidays, special days). |
| `fare_attributes.txt` | Fare details for different fare classes. |
| `fare_rules.txt` | Rules linking fares to routes or zones. |
| `feed_info.txt` | Metadata about the GTFS feed, including version and publisher. |
| `routes.txt` | Bus routes with IDs, names, and types. |
| `shapes.txt` | Polylines defining the paths of trips along routes. |
| `stops.txt` | Bus stop locations (latitude, longitude) and IDs. |
| `stop_times.txt` | Scheduled stop times for each trip along a route. |
| `trips.txt` | Individual trips associated with a route and shape. |

---

## Purpose in Analysis
- **stops:** Extract geographic coordinates for spatial joins with neighbourhoods.  
- **routes, stops, calendar, calander_dates, trips** Merged together to reconstruct the schedule for a given day. 

These datasets collectively allow the creation of predictive features for machine learning models targeting fare evasion.

---
