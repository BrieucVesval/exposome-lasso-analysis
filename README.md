# Exposome and Child Neurobehaviour: LASSO and Group LASSO (R)

Statistical-learning analysis of how early-life environmental exposures relate to children's neurobehavioural functioning. The exposures cover air pollution, built environment, metals, PFAS, pesticides, lifestyle and more.

📄 **Full report: [`docs/report.pdf`](docs/report.pdf)** (13 pages)

*Student project, CentraleSupélec, June 2024. Not maintained. Notebooks are commented in French.*

## Objective
Among 222 correlated pre- and post-natal exposures, find those associated with child neurobehaviour (`hs_Gen_Tot`, CBCL scale, children aged 6–11), after adjusting for covariates.

## Data
Simulated data from the ATHLETE project, based on the HELIX study: 1,301 mother–child pairs from six European cohorts (France, Greece, Lithuania, Norway, Spain, United Kingdom). This dataset is publicly available for the exposome data challenge.

## Method
1. **Pre-processing and exploratory analysis**: distributions, correlations, Cramér's V, PCA (`Traitement_donnees`, `EI_Exposomes`, `EI_Distributions`, `EI_ACP`).
2. **Covariate selection**: univariate tests, then stepwise selection with `MASS` (`Choix_Covariables`).
3. **Exposome-wide univariate regressions**: one linear model per exposure, adjusted for covariates (`EI_pvalues_Reglin`, `Exposome_Monovarie`).
4. **LASSO**: `glmnet::cv.glmnet` at λ_min, with covariates forced into the model (`EI_pvalue_Lasso`, `Exposome_Lasso`).
5. **Group LASSO**: `gglasso`, with groups given by exposure family (`GroupLasso`).
6. Breakdown of the selected exposures by family, sub-family and period (pregnancy vs. postnatal).

## Results
| Model | R² | RMSE | MAE |
|---|---|---|---|
| Linear regression (univariate pipeline) | 0.161 | 0.821 | 0.628 |
| LASSO | 0.310 | 15.80 | 11.94 |

Metrics are in-sample (computed on the training data). The report quotes R² ≈ 0.27 for LASSO.

Main findings (see the report):
- The three methods highlight different families: **metals** for LASSO, **lifestyle** for the univariate approach, **air pollution** for group LASSO.
- Group LASSO had to be run on a 225-sample subset because of its computational cost.
- The LASSO deep-dive points to metals (Cu, Mn, Pb), diet, and postnatal exposure to green spaces (NDVI). These findings are consistent with the literature. Full tables are in `results/` and charts in `figures/`.

![LASSO families](figures/top5/Lasso/5fam_Lasso.png)

## Getting started
The dataset is not included in this repository. Put `exposome.RData` (with the `exposome`, `covariates` and `phenotype` tables) in `data/`, then knit the notebooks in `notebooks/`.

```r
install.packages(c("dplyr","ggplot2","glmnet","gglasso","MASS","corrplot","vcd",
                   "RColorBrewer","broom","tibble","tidyr","readr","stringr","pacman"))
```

## Team
Irina Bran, Timo Descazeaud, Hippolyte Ducatillon, Theo Fontaine, Brieuc Vesval.
