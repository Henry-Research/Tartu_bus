# Maximizing Fare Evasion Capture within the Tartu Inner-City Bus Network
### Authors: Henry Chapman & Júlia Amaral

## Overview
Fare evasion remains a persistent challenge within Tartu’s public bus network. Although the city has implemented an electronic ticketing system, an estimated 10% of all rides occur without a valid fare, corresponding to ≈ $500,000 in lost revenue annually. Beyond financial loss, fare evasion undermines fairness for paying riders and increases pressure to raise ticket prices.
To mitigate this, the city deploys a mobile ticket-inspection van that selects bus stops throughout the day and inspects passengers as buses arrive. However, with only one inspection unit available, inspection efficiency is highly variable. Fare-evasion rates differ substantially across stop, route, and time of day, making strategic deployment essential.
The Tartu city government is interested in understanding the feasibility of proactively predicting fare evasion and maximising fare evasion capture via route optimization. This code represents an exploratory investigation into these topics.

## Goal
The objective of this project is to increase the efficiency of the ticket-inspection system by:
Identifying patterns of fare evasion across the Tartu bus network


- Quantifying spatial and temporal differences in evasion rates

- Developing data-driven strategies to direct inspectors

- Maximizing the expected number of evaders caught per inspection visit


All analyses, figures, and ML models in this repository directly support these goals. However, the repository does not represent a fully optimized inspection-routing system. Further work is required to improve fare-evasion prediction accuracy and to develop a more robust, end-to-end route-optimization framework for inspection scheduling. However, given the exploratory objective provided by the Tartu city government, the analysis still represents a successful project. 


## Repository structure 
The repository follows a clear and consistent structure:

Within each folder within “Notebooks” are a varying number of “pipes” ; these pipes represent the order the code is to be run in within that folder. The identical naming scheme of pipes is also found within each folder within “Output” which contains the output from their respective pipe. 

Within each folder within “Raw_data” are multiple data sets, which in the case of "Ticket_inspections" and “Validations_by_stops” represents temporal data. The “gtsf” data base contains a variety of data pertaining to all of the governmental run buses within Estonia. 

Every data set within “Raw_data” has its associated README file which briefly outlines the nature of the data

Project_final/
│
├── Notebooks/
│   ├── 1_Compiling_data/
│   ├── 2_Granularity_tuning/
│   ├── 3_ML_performance/
│   ├── 4_Hyperparam_tuning_XGBoost/
│   ├── 5_Predicting_future_date/
│   ├── 6_Prize_orientated_program/
│   ├── 7_Statistical_analysis/
│   └── 9999_Archived/
│
├── Output/
│   ├── 1_Compiling_data/
│   ├── 2_Granularity_tuning/
│   ├── 3_ML_performance/
│   ├── 4_Hyperparam_tuning_XGBoost/
│   ├── 5_Predicting_future_date/
│   └── 6_Prize_orientated_program/
│
└── Raw_data/
    ├── gtfs/
    ├── Neighbourhoods/
    ├── Ticket_inspections/
    └── Validations_by_stops/


## Overview of Data Sources and Analytical Purpose

This section provides a brief overview of the analysis performed and the purpose of each dataset used in the project.
The central aim of the analysis is to build a generalizable machine-learning model capable of predicting fare evasion at a specific bus stop during a defined time window.
To achieve this, several complementary datasets are integrated.


Each dataset contributes different features that aim to strengthen the predictive performance of the ML model.

**Ticket_inspections**
Ticket_inspections contains approximately 10 years of inspection records collected by the mobile ticket-inspection unit.
 For each inspection event, the dataset includes:
- Bus stop
- Bus line
- Date
- Time
- Number of fare evaders caught


This dataset provides the target variable for the ML models:
 ➡ Number of fare evaders detected during an inspection.
 
Because inspections are sparse relative to system-wide traffic, this dataset forms the backbone of the supervised learning framework.

**Ticket_validations**
Ticket_validations contains all ticket-scan events across the entire Tartu inner-city bus network. For each scan it records:
- Bus line
- Bus stop
- Date
- Time

From this dataset, two key predictive features are engineered:
Stop popularity : Represented as the number of individuals scanning onto buses at each stop, binned by day of week, and time of day (granularity explored in: 2_Granularity_tuning and 3_ML_performance).

Route density per stop : The baseline number of different bus lines that serve a given stop.

These features aim to capture passenger flow dynamics and network structure.

DUE TO THE SIZE of the Ticket_validations data set (> 1 Gbp) it cannot be uploaded to git, instead a subset of the data can be found at /validations_by_stops_subset.csv

**GTFS**
gtfs/ contains the General Transit Feed Specification datasets describing the full Tartu (and broader Estonian) bus network.
Relevant components include:
- Stop locations (latitude/longitude)
- Route definitions
- Stop–route relationships

From the GTFS data, we extract geographical coordinates for each stop.

**Neighborhoods**
Neighborhoods/ contains the GIS shapefile/GeoJSON defining Tartu’s neighborhood identityregions.
The GTFS stop coordinates are spatially joined to this file, enabling the model to use neighborhood identity as a predictive feature.


## Data mining approaches

Data Mining Approaches

The project applies a structured, multi-step data mining pipeline to predict fare evasion and optimize inspection targeting. The main approaches are outlined below:

1. Data Compilation, Preprocessing, Cleaning and Extracting

- Applying logical functions in order to compile "ticket_inspections" into a more usable and logical format.

- Cleaned and standardized timestamps, stop identifiers, and line codes.

- Performed logical operations for imputations of missing data including pure logical based and a k-nearest neighborhood approach.


2. Feature Engineering

- Merged multiple heterogeneous datasets in an attempt to increase predictive performance 

- Including spatial locations based on neighborhood binning.

- Temporal features : Time-of-day, day-of-week.

- Stop popularity based on baseline coverage.


3. Exploratory Data Analysis (EDA)

- Evaluationg temporal fare evasion.

- Investigating fare evasion rate within stop codes. 


4. Machine Learning Approaches

- Model Selection: Tested multiple supervised models for fare evasion prediction:

- Random Forest (RF)
- XGBoost
- Multi-layer Perceptron regression
- Multi-layer Perceptron classification
  
- Performance Evaluation: Used metrics such as RMSE, R², and auc_roc to compare models.

- Hyperparameter Tuning: Performed intelligence hyper parameter tuning with OPTUNA on XGBoost, to optimize predictive accuracy.


5. Investigating granularity importance 

- Explored different temporal and spatial granularities to maximize prediction performance.


6. Route Optimization (Prize-oriented Program)

- Developed a reward inspection routing program to suggest sequences of stops that maximize expected fare evaders caught.

- Integrated predicted evasion counts, travel distances, and inspection durations.

- Not a fully optimized system, but serves as a proof-of-concept for data-driven inspection scheduling.


7. Archival / Exploratory Notebooks

- Maintained a 9999_Archived folder to document:

- Alternative modeling approaches

- Failed attempts or dead ends

- Iterative improvements during feature engineering and model tuning

- This pipeline combines data preprocessing, feature engineering, exploratory analysis, predictive modeling, and optimization in an attempt to deliver actionable insights into fare evasion patterns and inspection strategy.

- After the lack-luster results of the ML process, a simplistic statistical approach was breifly explored in 7_Statistical_analysis/


## Reproducibility & System Requirements
The analysis was performed on:
- HP Z440 workstation
- 128 GB RAM
- 21-core CPU
- NVIDIA Quadro K2200

Most steps run efficiently on typical modern computers. The only exception is the large ticket validations dataset, which may require chunked reading on machines with <16 GB RAM.

## To Replicate the Analysis
Clone or download the repository:

- git clone https://github.com/Henry-Research/Tartu_bus.git
- Adjust file paths inside the notebooks to match your local directory structure.

Run the notebooks in numerical order, beginning with the 01_ folder.


All outputs (figures, processed dataframes, and ML models) will populate automatically in the matching subdirectories under Output/.


The repository includes sufficient explanations throughout the codebase to help any experienced programmer understand:
- where each piece of analysis is performed.
- how the notebooks connect.
- what each stage of the pipeline accomplishes.


Some notebooks produce intermediate or exploratory results not shown in the final poster—these are included intentionally to demonstrate the full scope of work completed.

The 9999_Archived folder contains exploratory notebooks created throughout the development of the project. These notebooks document early experiments, alternative modelling attempts, and analytical directions that were ultimately revised or replaced.

They are included to illustrate the complex and iterative nature of the project, as well as the methodological challenges and dead ends encountered along the way. While not part of the final analysis pipeline, they provide valuable context regarding the reasoning, trial-and-error, and decision-making that shaped the final models and workflow.


