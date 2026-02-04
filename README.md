# Subnational Migration Analysis
This repository provides the computational framework and analytical pipeline for modeling subnational migration flows by disaggregating national level flows to specific demographic groups on a subnational levels. 
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
To account for the noise of daily migration rates on the subnational scale and specific demographic groups, we use a cumulative flow fitting method. Therefore, instead of averaging daily counts, we estimate the intensities $\lambda_i§ and $\gamma_i$ given by

$$\hat{\lambda} = \frac{6 \sum_{t=1}^{n} t M_t}{n(n+1)(2n+1)}$$

**Where:**
* $n$ is the total number of days in the observation period.
* $t$ is the specific day index.
* $M_t$ is the cumulative count of arrivals or exits up to day $t$.

### 3. Code
This repository provides the processed subnational datasets and the scripts required to reproduce the analysis.
