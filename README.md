## Introduction

This project applies Bayesian regression and hierarchical modeling to a small agricultural experiment investigating the effect of a growth regulator on seed production in two varieties of oilseed rape.

The experiment followed a completely randomized design with four treatment combinations formed by:

- growth regulator applied or not applied;
- oilseed variety B or N.

Each treatment combination was replicated six times, giving a total sample size of 24 plots.

The response variable is **Thousand Grain Weight (TGW)**, the weight of 1,000 randomly selected seeds. During the growing season, some plots were grazed by pigeons. The percentage of each plot affected by grazing was therefore recorded as **Damage** and included as a covariate because it could influence seed development.

The main research question is:

**How does TGW differ between treatment combinations after accounting for pigeon damage?**

### Exploratory Analysis

The dataset is complete, with no missing observations. TGW ranges from approximately 3.14 to 5.02, while Damage ranges from 0% to 60%.

Exploratory plots indicate differences in TGW between treatment groups and a negative relationship between grazing damage and grain weight, motivating the inclusion of both treatment and Damage in the statistical models.

### Bayesian Models

Three Bayesian regression models of increasing complexity are fitted:

1. **Fixed-effect ANCOVA**
   - treatment-specific fixed effects;
   - common Damage slope across treatment groups.

2. **Hierarchical model with random intercepts**
   - partially pooled treatment intercepts;
   - common Damage slope.

3. **Hierarchical model with random intercepts and random slopes**
   - partially pooled treatment intercepts;
   - treatment-specific Damage slopes.

Weakly informative, data-scaled priors are assigned to regression coefficients and variance parameters.

All models are fitted using Hamiltonian Monte Carlo with the No-U-Turn Sampler (NUTS) through `brms`/Stan.

### Model Assessment

Model quality is assessed using:

- posterior predictive checks;
- residual diagnostics;
- trace plots;
- autocorrelation diagnostics;
- \(\hat{R}\);
- effective sample size (ESS);
- Bayesian \(R^2\).

The chains show good convergence and mixing, with \(\hat{R}\) values close to 1 and sufficiently large effective sample sizes.

### Model Comparison

The competing models are compared using Leave-One-Out Cross-Validation (LOO-CV).

The three treatment/Damage models show similar predictive performance and clearly outperform a null model containing neither treatment nor Damage. Because their predictive performances are not meaningfully distinguishable, the simpler fixed-effect ANCOVA model is selected for interpretation.

### Posterior Treatment Comparisons

Posterior treatment contrasts are estimated using Estimated Marginal Means while controlling for Damage.

The analysis indicates that:

- the growth regulator does not show a meaningful effect on TGW;
- variety N has higher TGW than variety B;
- increasing pigeon damage is associated with lower TGW.

These conclusions remain tentative because the experiment contains only 24 observations and the Damage variable was rounded to the nearest 10%.

---

## Summary

**Bayesian Modeling – Thousand Grain Weight Experiment:** Built and compared Bayesian ANCOVA and hierarchical models (`brms`, `Stan`) for treatment effects on oilseed-rape grain weight while controlling for grazing damage. Used weakly informative priors, NUTS sampling, posterior predictive checks, Bayesian \(R^2\), MCMC convergence diagnostics, LOO cross-validation, and posterior pairwise treatment contrasts (`emmeans`).