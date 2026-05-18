# Operational Risk Modeling — LDA Approach

University project for the course **Credit and Operational Risk Methods**.  
Implementation of the **Loss Distribution Approach (LDA)** under the AMA framework to calculate annual **99.9% OpVaR** and **OpES** for a bank.

## Overview

The project models operational risk using internal loss data from two selected business lines (out of seven available in the dataset):
- **Buss_Distr** — Business Disruption
- **Com_Ban** — Commercial Banking

The analysis follows the standard LDA workflow: fit a frequency distribution, fit a severity distribution, aggregate via Monte Carlo simulation, and read OpVaR / OpES off the simulated loss distribution.

## Methodology

1. **Exploratory Data Analysis** — frequency and severity of operational losses per business line  
2. **Frequency distribution** — Negative Binomial (chosen based on variance > mean), validated with a chi-squared goodness-of-fit test  
3. **Severity distribution** — compared Lognormal, Weibull, and Exponential using the Kolmogorov–Smirnov test:
   - Buss_Distr → **Weibull** (shape = 1.016, scale = 29,298.63)
   - Com_Ban → **Lognormal** (μ = 10.097, σ = 0.928)
4. **Monte Carlo simulation** — 10,000 annual scenarios of aggregated losses  
5. **Risk measures** — 99.9% OpVaR and OpES from the simulated loss distribution

## Results

| Measure        | Value         |
|----------------|---------------|
| OpVaR (99.9%)  | 2,542,605     |
| OpES (99.9%)   | 2,900,967     |

## Files

- `Projekt_LDA.Rmd` — main R Markdown source file
- `Projekt_LDA.html` — compiled report
- `plik7.csv` — input dataset (gross losses by year and business line, 1989–2023)

## Requirements

R packages:
```r
install.packages(c("dplyr", "ggplot2", "MASS", "knitr", "kableExtra", "rmarkdown"))
```

## Notes

The model demonstrates a standard LDA implementation along with a discussion of its known weaknesses — lack of comparability between banks, sensitivity to distribution choice, data scarcity in the tail, and difficulty of backtesting at the 99.9% level — which led the Basel Committee to replace AMA with the Standardised Measurement Approach (SMA).
