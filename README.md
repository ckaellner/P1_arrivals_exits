# Subnational Migration Analysis
This repository provides the computational framework and analytical pipeline for modeling subnational migration flows by disaggregating national level flows to specific demographic groups on a subnational levels. 
By treating arrivals and exits as independent Poisson processes, we utilize the Skellam Distribution to analyze net migration dynamics across various demographic and spatial dimensions.

This repository is the codebase for the paper:  
> **"Arriving Young, Leaving Old(er): Age-Structured International Migration on Subnational Scale in Austria"** > *Available as a preprint on [arxiv](https://arxiv.org/abs/2511.19007v1).*



## Methodology
### 1. Data Disaggregation
Migration data across three key dimensions:
- **Demographic:** Specific age-cohorts (Age Groups).
- **Nationality:** Top 25 Diaspora populations (by citizenship).
- **Spatial:** Subnational units (Municipalities and State-Urbanisation-Classification).

### 2. Modelling
We assume that arrivals ($A$) and exits ($E$) for a given unit follow independent Poisson processes:
$$A \sim \text{Poisson}(\lambda)$$
$$E \sim \text{Poisson}(\gamma)$$

The net migration $N = A - E$ is therefore modeled by a **Skellam distribution**. This allows us to estimate:
- **$\lambda$ (Lambda):** Daily intensity of arrivals.
- **$\gamma$ (Gamma):** Daily intensity of exits.
- **Net Flow:** $\lambda - \gamma$.

### 3. Parameter Estimation
Parameters are estimated using a cumulative flow fitting method (integrated into `f_skellam_analysis`), ensuring the model captures long-term trends while accounting for daily variance.
