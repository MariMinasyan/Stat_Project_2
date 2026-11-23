# How Top-Heavy Is Global Wealth? A Pareto Model for the Forbes Billionaires of 2021

**Course:** AUA CS 108 Statistics — Fall 2026, Project 2  
**Authors:** Mari Minasyan, Aida Kirakosyan, Liana Petrosyan  
**Date:** November 2026

## Project overview

This project studies how concentrated wealth is among the world's billionaires using the **Forbes Billionaires of 2021** dataset. We model net worth above a fixed threshold of **$4 billion USD** with a Pareto Type I distribution.

The main parameter of interest is the Pareto tail index, \(\alpha\). We estimate it and use it to study:

- the share of total billionaire wealth owned by the richest 1%, 5%, 10%, 20%, and 50%;
- the probability that a billionaire's net worth exceeds selected thresholds;
- the Gini coefficient within the billionaire population;
- whether the model is compatible with the 80/20 rule.

The report also compares several estimators, constructs confidence intervals, and uses simulation to examine estimator properties and interval coverage.

## Repository contents

```text
.
├── Report.Rmd
├── README.md
└── Billionaire.csv       # Download separately; not included by default
```

## Dataset

The analysis uses the **Forbes Billionaires of 2021** dataset published on Kaggle:

- [Kaggle dataset — Forbes Billionaires of 2021](https://www.kaggle.com/datasets/roysouravcu/forbes-billionaires-of-2021)
- [Forbes World's Billionaires](https://www.forbes.com/billionaires/)

Download the CSV and place it in the same directory as `Report.Rmd`. The default expected filename is:

```text
Billionaire.csv
```

If that exact filename is not present, the R Markdown file also searches the working directory and its subdirectories for a CSV filename matching `forbes*.csv`.

The report identifies the net-worth column by looking for a column name containing `net worth`, `networth`, or `wealth`. It uses the country column for an optional supplementary comparison when one is available.

## Requirements

Install **R** and **RStudio** (or another environment that supports R Markdown).

The report uses base R functions and the following R packages:

- `rmarkdown` — renders the report;
- `knitr` — formats tables and processes R Markdown code chunks.

Install the required packages if they are not already installed:

```r
install.packages(c("rmarkdown", "knitr"))
```

To render the PDF version, a LaTeX distribution is also required. One option is TinyTeX:

```r
install.packages("tinytex")
tinytex::install_tinytex()
```

## How to run

1. Download the dataset and put the CSV next to `Report.Rmd`.
2. Open `Report.Rmd` in RStudio.
3. Select **Knit** to render the report, or run one of the commands below from the project directory.

Render HTML:

```r
rmarkdown::render("Report.Rmd", output_format = "html_document")
```

Render PDF:

```r
rmarkdown::render("Report.Rmd", output_format = "pdf_document")
```

The output file is created in the same directory as `Report.Rmd` unless another output directory is specified.

## Analysis workflow

The report is organized into the following parts:

1. **Problem description** — defines the research question, parameter of interest, and assumptions.
2. **Pareto model** — introduces the Pareto Type I distribution and derives the concentration measures used in the analysis.
3. **Dataset preparation** — imports the Forbes data, converts net worth values to numeric values in billions of USD, removes missing values, and keeps observations with net worth of at least $4 billion.
4. **Model diagnostics** — examines the Pareto assumption using a log-transformed histogram, log-log survival plot, QQ plot, empirical and fitted CDFs, and a parametric-bootstrap Kolmogorov–Smirnov test.
5. **Estimation** — constructs and compares the maximum likelihood estimator (MLE), unbiased estimator (UMVUE), minimum-MSE estimator, and method-of-moments estimator. It also studies their properties analytically and by simulation.
6. **Confidence intervals** — constructs exact, Wald, log-Wald, and likelihood-ratio intervals, and evaluates interval coverage through simulation.
7. **Conclusions** — interprets the estimates and discusses assumptions and limitations.

## Simulation settings

The main simulation settings are defined near the beginning of `Report.Rmd`, in the global settings chunk:

```r
SEED      <- 2026
B_SIM     <- 5000
B_BOOT    <- 1000
LEVEL     <- 0.95
DATA_FILE <- "Billionaire.csv"
```

- `SEED` makes the simulations reproducible.
- `B_SIM` is the number of replications used in the estimator and confidence-interval simulation studies.
- `B_BOOT` is the number of bootstrap replications used for the goodness-of-fit test.
- `LEVEL` sets the confidence level.
- `DATA_FILE` sets the expected dataset filename.

Change these values only if you want to run the analysis with different settings. Increasing the number of replications can make simulation results more stable, but also increases runtime.

## Important assumptions and limitations

- The analysis treats billionaire net worth observations above $4 billion as independent and identically distributed Pareto observations. This is a modelling approximation.
- The Forbes list is close to a census of the selected billionaire population, rather than a random sample.
- Some fortunes may be related through family ownership or shared companies.
- Forbes net-worth figures are estimates and are rounded to $0.1 billion.
- The Pareto model is intended to describe the upper tail and may not fit the entire wealth distribution equally well.
- The conclusions describe wealth **among billionaires in the 2021 list**, not wealth inequality across the entire world population.
- The dataset is a snapshot of 2021; results may differ for other years.

## References

- Forbes (2021). *The World's Billionaires 2021.* https://www.forbes.com/billionaires/
- Roy, S. *Forbes Billionaires of 2021.* Kaggle. https://www.kaggle.com/datasets/roysouravcu/forbes-billionaires-of-2021
- Pareto, V. (1896). *Cours d'économie politique.*
- Newman, M. E. J. (2005). “Power laws, Pareto distributions and Zipf's law.” *Contemporary Physics*, 46(5), 323–351.
- Clauset, A., Shalizi, C. R., & Newman, M. E. J. (2009). “Power-law distributions in empirical data.” *SIAM Review*, 51(4), 661–703.
- Arnold, B. C. (2015). *Pareto Distributions* (2nd ed.). CRC Press.
- Casella, G., & Berger, R. L. (2002). *Statistical Inference* (2nd ed.). Duxbury.
