# Subnational Migration Analysis [![DOI](https://zenodo.org/badge/1149565194.svg)](https://doi.org/10.5281/zenodo.18507927)
This repository provides the computational framework and analytical pipeline for modelling subnational migration flows of specific demographic groups and on a subnational geographical level. 
By treating arrivals and exits as independent Poisson processes, we utilize the Skellam Distribution to analyze net migration dynamics across various demographic and spatial dimensions.

This repository is the codebase for the paper:  
> **"Arriving Young, Leaving Old(er): Age-Structured International Migration on Subnational Scale in Austria"** > *Available as a preprint on [arxiv](https://arxiv.org/abs/2511.19007v1).*

Migration data is disaggregated across three key dimensions:
- **Demographic:** Specific age-cohorts (Age Groups).
- **Nationality:** Top 25 Diaspora populations (by citizenship).
- **Spatial:** Subnational units (municipalities and state-urbanisation-classification).

> **Data Disclosure**: The analysis is based on a high-resolution, confidential dataset of individual-level administrative records. To comply with data protection regulations and confidentiality agreements, the raw micro-data is not public. However, this repository provides the weekly aggregated matrices used to generate the paper's findings, allowing for reproducibility of the results.

## Methodology
### 1. Modelling
We assume that arrivals ($A$) and exits ($E$) for a given unit follow independent Poisson processes:
$$A_i(t) \sim \text{Poisson}(\lambda_it)$$ and 
$$E_i(t) \sim \text{Poisson}(\gamma_it)$$.

The net migration $N = A - E$ is therefore modeled by a Skellam distribution. This allows us to estimate:
- **$\lambda_i$:** Daily intensity of arrivals.
- **$\gamma_i$:** Daily intensity of exits.
- **$\lambda_i - \gamma_i$:** Net daily migration flow.

### 2. Parameter Estimation
To account for the noise of daily migration rates on the subnational scale and specific demographic groups, we use a cumulative flow fitting method. Therefore, instead of averaging daily counts, we estimate the intensities $\lambda_i$ and $\gamma_i$ given by

$$\hat{\lambda} = \frac{6 \sum_{t=1}^{n} t M_t}{n(n+1)(2n+1)}$$

**Where:**
* $n$ is the total number of days in the observation period.
* $t$ is the specific day index.
* $M_t$ is the cumulative count of arrivals or exits up to day $t$.

## Code
This repository contains the R code used to analyze and visualize the intensity of arrivals and exits (flows) across different demographic categories such as nationality and age. The provided script works as follows:

### 1. Functions
#### a. Mathematical Functions
* **`f_poisson_fitting(weekly_counts)`**: Calculates a daily intensity rate from a vector of weekly arrival/exit counts.
* **`f_skellam_analysis(arrival_matrix, exit_matrix)`**: Processes entire matrices of arrival and exit data. It returns a structured data frame containing the daily arrival rate ($\hat{\lambda}_i$), daily exit rate ($\hat{\gamma_i}$), and the net flow.

#### b. Visualizations
* **`plot_arrivals_exits(skellam_analysis)`**: Scatter plot that compares arrival intensity against exit intensity. 
    * Points below the **dashed identity line** ($\hat{\lambda}=\hat{\gamma}$) indicate units with positive net growth.
* **`plot_age_structure(skellam_analysis)`**: A mirrored line plot specifically designed for age-based data (ages 2–100).
    * **Blue dashed line**: Positive arrival intensity ($\hat{\lambda}_i$).
    * **Red dashed line**: Negative exit intensity ($-\hat{\gamma}_i$).
    * **Solid Grey line**: The Net flow, highlighting which age groups are growing or shrinking.

### 2. Data
The scripts are designed to work with CSV files stored in a "git_data/" folder provided in this repository. By running the entire code, data stored in the same structure as in this repository will be called by the code into the R environment.

### 3. Analysis
To run the analysis, call the **`f_skellam_analysis(arrival_matrix, exit_matrix)`** function with the correpsonding datasets. The resulting dataframes for age and nationality can be visualized using **`plot_arrivals_exits(skellam_analysis)`** and **`plot_age_structure(skellam_analysis)`**; for age-cohort and state-urbanisation specific analysis no specific visualization is provided
