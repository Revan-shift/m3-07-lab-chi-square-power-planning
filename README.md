![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Chi-Square and Power Planning

## Overview

Not every question in data science involves numerical measurements. When your data are categorical — survey responses, product choices, segment labels — the chi-square family of tests is your go-to tool for asking "Does the observed distribution match what we expected?" and "Are these two categorical variables related?"

Knowing that a result is statistically significant is only half the story, though. You also need to know how big the effect is (Cramér's V) and whether your study was even large enough to detect it (statistical power). Planning sample sizes before running an experiment saves time, money, and the frustration of inconclusive results.

In this lab you will work through both sides of that equation. First, you will run chi-square tests on realistic categorical data and quantify effect sizes. Then, you will build power simulations to see how sample size influences your ability to detect an effect, and apply that knowledge to plan an A/B test. The lab finishes with a practical decision memo — the kind of artefact you would hand to a product manager or research lead.

## Learning Goals

By the end of this lab, you should be able to:

- Perform a chi-square goodness-of-fit test and interpret the result
- Perform a chi-square test of independence on a contingency table
- Compute Cramér's V and classify the effect size (small / medium / large)
- Simulate statistical power for a chi-square test across a range of sample sizes
- Calculate the required sample size for a two-proportion A/B test
- Communicate sample-size recommendations and test results in a practical decision memo

## Setup and Context

You will work in a single Jupyter notebook. The lab is divided into two halves: **chi-square testing** (Tasks 1–3) and **power & sample-size planning** (Tasks 4–6). All code, plots, and written analysis go in the same notebook.

### Prerequisites

- Understanding of chi-square tests, expected frequencies, and degrees of freedom (covered in the lesson)
- Familiarity with the concept of statistical power and Type II error
- Comfortable with NumPy, pandas, and basic plotting

## Requirements

1. **Fork** this repo to your GitHub account, then **clone** your fork locally.
2. Make sure you have the following Python packages installed:

```bash
pip install numpy pandas scipy matplotlib seaborn statsmodels
```

3. Work inside the notebook specified in **Getting Started**.

## Getting Started

1. Create a new Jupyter notebook called **`m3-07-chi-square-power-planning.ipynb`** in the root of your cloned repo.
2. At the top of the notebook, import the libraries you will need:

```python
import numpy as np
import pandas as pd
import scipy.stats as st
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.stats.proportion import proportions_ztest, proportion_effectsize
from statsmodels.stats.power import NormalIndPower

np.random.seed(42)
```

3. Work through the tasks below in order. Give each task its own Markdown heading in the notebook.

## Tasks

### Task 1: Chi-Square Goodness-of-Fit Test

**Scenario:** A mobile-game studio expects that players choose among four character classes in equal proportions (25 % each). After a recent update, they collected the following counts from 400 new players:

| Class | Warrior | Mage | Rogue | Healer |
|-------|---------|------|-------|--------|
| Count | 120     | 85   | 110   | 85     |

1. Store the observed counts in a NumPy array.
2. Compute the expected counts under the null hypothesis of equal preference.
3. Run `st.chisquare(observed, f_exp=expected)`.
4. Report the χ² statistic, degrees of freedom, and p-value.
5. At α = 0.05, state whether you reject or fail to reject H₀.
6. In a Markdown cell, answer: "Which class (if any) appears over- or under-represented, and by how much?"

### Task 2: Chi-Square Test of Independence

**Scenario:** A marketing team wants to know whether subscription tier (Free, Basic, Premium) is associated with churn status (Churned, Retained). They collected data from 600 customers:

| | Free | Basic | Premium |
|---------|------|-------|---------|
| Churned | 90   | 60    | 30      |
| Retained| 110  | 140   | 170     |

1. Create the contingency table as a 2 × 3 NumPy array or pandas DataFrame.
2. Run `st.chi2_contingency(table)` and unpack χ², p-value, degrees of freedom, and expected frequencies.
3. Display the expected-frequency table.
4. Report the χ² statistic, df, and p-value.
5. State your decision and a plain-language interpretation (e.g. "There is / is not evidence of an association between subscription tier and churn.").

### Task 3: Compute Cramér's V

Using the χ² statistic and the contingency table from Task 2:

1. Write a function:

```python
def cramers_v(chi2, n, min_dim):
    """
    Returns Cramér's V.

    Parameters
    ----------
    chi2 : float
        Chi-square statistic.
    n : int
        Total number of observations.
    min_dim : int
        min(rows, cols) - 1 of the contingency table.
    """
```

2. Compute Cramér's V for the Task 2 result.
3. Classify the effect size using Cohen's benchmarks for df* = min(rows, cols) − 1:
   - Small ≈ 0.10, Medium ≈ 0.30, Large ≈ 0.50 (for 2 × 3 tables, df* = 1)
4. In a Markdown cell, answer: "Is the relationship between tier and churn weak, moderate, or strong? What does this mean for the marketing team?"

## Submission

### What to submit

- `m3-07-chi-square-power-planning.ipynb` — your completed Jupyter notebook containing all code, plots, and written analysis.

### Definition of done (checklist)

- [ ] Chi-square goodness-of-fit test correctly implemented and interpreted
- [ ] Chi-square test of independence correctly implemented with expected-frequency table
- [ ] Cramér's V computed and effect size classified
- [ ] Notebook runs top-to-bottom without errors (`Kernel → Restart & Run All`)

### How to submit (Git workflow)

```bash
git add .
git commit -m "lab: chi-square and power planning"
git push origin main
```

Then open a **Pull Request** from your fork back to the original repo. Paste the PR link in the student portal.
