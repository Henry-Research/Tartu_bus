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


All analyses, figures, and ML models in this repository directly support these goals. However, the repository does not represent a fully optimized inspection-routing system. Further work is required to improve fare-evasion prediction accuracy and to develop a more robust, end-to-end route-optimization framework for inspection scheduling. However, given the exploratory objective provided by the Tartu city government, the analysis still represents a successful project. Please read the commentary found within the final header "Comments on Approach Taken – Limitations, Regrets, and Future Directions" on a more detailed discussion the limitatatoins and regrets of the approach, and a discussion of future steps. 


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
│   └── 7_Statistical_analysis/
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

7. Statistical approach

- A breif delve into a old-school statistical based approach
- Identifying bus stops with a high number of mean unvalidated indivudals with high confidence scores


This pipeline combines data preprocessing, feature engineering, exploratory analysis, predictive modeling, and optimization in an attempt to deliver actionable insights into fare evasion patterns and inspection strategy.


After the lack-luster results of the ML process, a simplistic statistical approach was breifly explored in 7_Statistical_analysis/


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

They are included to illustrate the complex and iterative nature of the project, as well as the methodological challenges and dead ends encountered along the way. While not part of the final analysis pipeline, they provide valuable context regarding the reasoning, trial-and-error, and decision-making that shaped the final models and workflow.



## Comments on Approach Taken – Limitations, Regrets, and Future Directions

This project is an excellent example of the exploratory nature of data science. Not every methodological choice ends up being optimal, but each step contributes valuable insights. Several aspects of the approach taken—especially around temporal modelling and spatiotemporal reduction—deserve comment, clarification, and reconsideration.

1. Temporality Appears to Have Very Limited Impact

The results from /Notebooks/ML_performance/Pipe2 show that incorporating temporal features improves machine-learning performance very little. Similarly, the output from /Notebooks/7_Statistical_analysis/Pipe1 demonstrates that including temporal features barely changes the coefficient of variation. Together, these observations suggest that the way temporality was encoded here is either:

a) Irrelevant, or
b) Not modelled in the right way.

If (a) is true, it challenges one of the project’s foundational assumptions—that fare evasion is temporally structured in a way that meaningfully affects prediction.

However, /Notebooks/1_Compiling_data/Pipe3 clearly shows that fare evasion does have temporal structure. The key assumption we made was that each bus stop has its own unique temporal × fare-evasion relationship. If this assumption is false—i.e., if all stops share a broadly similar temporal pattern—then encoding temporality at the stop-level is unnecessary and will appear uninformative in modelling.

This is a major point that should have been investigated earlier.

2. Current Temporal Binning May Be Suboptimal
   
If scenario (b) is true, then our modelling approach is failing to capture the true temporal dimensionality of fare evasion. Uniform 30-minute and 60-minute bins may be too coarse, too arbitrary, or simply misaligned with human transit behaviour.

A more realistic approach might include:
- Morning / Midday / Afternoon / Evening bins
- Rush-hour vs non-rush-hour categorisation
- Data-driven segmentation based on peaks in ridershi
- Unequal time bins that reflect natural behaviour, not clock boundaries
  
These may better capture the actual temporal dynamics influencing fare evasion.

3. Limitations of the Spatiotemporal Reduction Approach
   
The spatial-temporal reduction used here increases statistical power by aggregating data at the neighbourhood level, but this comes at the cost of losing spatial resolution. Neighbourhoods are broad, and potentially obscure meaningful patterns between specific stops.

An improved approach could:
- Maintain unique stop IDs, while
- Adding neighbourhood (or sub-neighbourhood) as a hierarchical feature, rather than a replacement
- Or define custom spatial clusters that better balance power and resolution
  
This would preserve fine-grained stop-level variation while still benefiting from aggregated features.

4. Additional Features and Alternative Approaches
    
If repeating the analysis, the following should be prioritised:

a)More detailed investigation of temporal dimensionality
- Test whether temporal features genuinely improve prediction in either ML or statistical frameworks.

Explore non-uniform or behaviourally informed time-bins.
b) Consider fully statistical approaches
- It may be possible to model fare evasion using classical statistical methods if temporal differences are subtle but systematic.

c) Incorporate passenger-flow features from the ticket-validations dataset
- A heatmap of ticket validations over time would reveal the flow of passengers through the city.
- 
For example:
Morning: peaks at peripheral stops
Evening: flows concentrate toward central stops

Because the validations dataset includes bus line IDs, we can estimate how passengers enter the network and approximate their likely movement patterns between stops.

This could become a highly informative feature when modelling both fare evasion and optimal inspection routing.

11. Route Optimisation Algorithm – Future Work

The current routing method is functional but far from optimal. Future versions should rely on OR-Tools, which provides industrial-grade algorithms for routing and optimisation.

The most challenging improvement will be designing a system that:
- Minimises predictable patterns
- Ensures inspectors take different routes on different days
- Avoids allowing fare evaders to learn the inspection schedule
- Capturing a large variety of bus lines
  
This requires optimisation across multiple days, possibly weeks, not just within a single day.
