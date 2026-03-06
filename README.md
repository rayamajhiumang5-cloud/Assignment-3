# Assignment-3
# SwiftCart Econometric Audit Project

## Overview

This project applies modern applied econometric techniques to audit several claims made by SwiftCart regarding driver compensation, delivery efficiency, and the impact of its premium loyalty program. The analysis focuses on situations where traditional parametric assumptions fail due to skewed distributions, outliers, and selection bias. The project therefore relies on non-parametric inference methods and causal inference techniques to obtain more reliable results.

The project is organized into four main analytical phases. Each phase addresses a specific empirical problem and implements a method appropriate for the statistical challenges present in the data.

---

## Phase 1: Bootstrapping Non-Parametric Uncertainty

The first phase investigates driver tip distributions, which are characterized by a large number of zero tips and a small number of high-value outliers. Because this distribution is zero-inflated and heavily right-skewed, classical inference based on the Central Limit Theorem is unreliable for small samples.

To address this issue, a manual bootstrap procedure was implemented. The dataset was resampled with replacement 10,000 times, and the median tip was calculated for each resample. The resulting bootstrap distribution was used to estimate the sampling variability of the median and to construct a 95 percent confidence interval using percentile methods.

The bootstrap interval was asymmetric, reflecting the underlying skewness of the data. This illustrates how non-parametric bootstrapping can capture uncertainty more accurately than symmetric parametric intervals.

---

## Phase 2: Non-Parametric Permutation Testing

The second phase evaluates the performance of SwiftCart’s "Batch Routing" delivery algorithm using simulated A/B test data. The control group follows a normal distribution of delivery times, while the treatment group follows a log-normal distribution that introduces extreme outliers caused by system crash loops.

Because these outliers violate the homoscedasticity assumption required for a standard t-test, a manual permutation test was constructed. The delivery times from both groups were pooled together and randomly permuted 5,000 times. Each permutation was split into two pseudo-groups, and the difference in mean delivery times was calculated.

The empirical p-value was obtained by measuring the proportion of permutations producing differences as extreme as the observed difference. This non-parametric approach provides a robust significance test that does not rely on distributional assumptions.

---

## Phase 3: Propensity Score Matching and Causal Inference

The third phase examines the claim that subscribers to the SwiftPass loyalty program spend significantly more per month. A naive comparison of average spending between subscribers and non-subscribers produces a Simple Difference in Outcomes (SDO). However, this estimate is biased because high-volume users are more likely to subscribe to the program, creating a classic selection bias problem.

To correct for this issue, Propensity Score Matching (PSM) was implemented. A logistic regression model was used to estimate the probability that a user subscribes to SwiftPass based on pre-treatment characteristics such as order volume, account age, and historical support tickets. Each subscriber was then matched with the nearest non-subscriber based on their estimated propensity score.

Using this matched sample, the Average Treatment Effect on the Treated (ATT) was calculated. The ATT provides a more credible estimate of the causal impact of the loyalty program because it compares users with similar pre-treatment characteristics.

---

## Phase 4: Covariate Balance Visualization (Love Plot)

To validate the quality of the matching procedure, a Love Plot was generated using seaborn and matplotlib. This visualization compares standardized mean differences for each covariate before and after matching.

Successful matching is indicated when the standardized mean differences for all covariates move close to zero and fall within the commonly accepted ±0.1 threshold. When this occurs, it suggests that the treated and control groups are balanced along observable characteristics and that selection bias has been substantially reduced.

However, it is important to note that this evidence only addresses bias arising from observed covariates. Unobserved confounders may still affect the estimated treatment effect.

---

## Technologies Used

- Python
- NumPy
- Pandas
- scikit-learn
- Matplotlib
- Seaborn

---

## Key Methods Implemented

- Manual Bootstrap Resampling
- Non-Parametric Permutation Testing
- Logistic Regression Propensity Score Estimation
- Nearest Neighbor Matching
- Standardized Mean Difference Diagnostics
- Love Plot Visualization

---

## Conclusion

This project demonstrates how modern econometric techniques can be used to analyze complex real-world data where traditional statistical assumptions fail. By combining non-parametric inference with causal inference methods, the analysis produces more credible and robust insights into SwiftCart’s operational claims and user behavior.
