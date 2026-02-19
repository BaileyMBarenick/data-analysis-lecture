# Data Analysis Lecture — Anscombe's Quartet

Lecture 12 (554) case study demonstrating why summary statistics must always be paired with visualization, using Anscombe's Quartet as the worked example.

---

## Repository Contents

| File | Description |
|------|-------------|
| `anscombe_quartet.tsv` | Raw dataset — 4 groups (I–IV), 11 observations each, tab-separated |
| `anscombe_analysis.ipynb` | Fully executed Jupyter notebook with all analysis and inline plots |
| `anscombe_plan.md` | Iterative analysis plan documenting decisions, results, and addenda |

---

## What is Anscombe's Quartet?

Anscombe's Quartet (F.J. Anscombe, 1973) is a set of four datasets that are nearly **identical across every standard summary statistic** yet look completely different when plotted:

| Property | Value (all four groups) |
|----------|------------------------|
| Mean x | 9.0 |
| Mean y | ≈ 7.50 |
| Variance x | 11.0 |
| Variance y | ≈ 4.12 |
| Correlation | ≈ 0.816 |
| Regression line | y ≈ 0.50x + 3.00 |
| R² | ≈ 0.67 |

Despite identical statistics, each group has a distinct structure:

- **Group I** — Classic linear relationship with random scatter; linear regression is appropriate
- **Group II** — Curved (quadratic) relationship; a linear model is structurally wrong
- **Group III** — Linear trend with a single high-leverage outlier that distorts the slope
- **Group IV** — All x values are 8 except one point at x = 19; the regression is entirely driven by that outlier

---

## Notebook Overview (`anscombe_analysis.ipynb`)

The notebook follows the plan laid out in `anscombe_plan.md` across two iterations:

### 1. Descriptive Statistics
Per-group mean, standard deviation (SD), and standard error of the mean (SEM) for both x and y, computed separately before any cross-group comparison.

### 2. Regression Analysis
Full OLS regression table for each group via `scipy.stats.linregress`:

| Statistic | Meaning |
|-----------|---------|
| `slope` | Rate of change of y per unit x |
| `SE(slope)` | Standard error of the slope estimate |
| `intercept` | y-value when x = 0 |
| `SE(intercept)` | Standard error of the intercept estimate |
| `R` | Pearson correlation coefficient |
| `R²` | Proportion of variance in y explained by x |
| `p(slope)` | Two-tailed p-value for the slope |

### 3. Scatter Plots with 95 % Confidence Band
2×2 grid showing each group's data with:
- OLS regression line (dashed)
- 95 % confidence band for the mean response
- Annotation box: equation, R², p-value

### 4. Mean ± SEM Bar Chart
Intentionally "misleading" chart showing all four groups as near-identical — illustrating what gets hidden when you skip visualization.

### 5. Residual Plots
y − ŷ per group, revealing:
- Group I: random scatter around zero (good fit)
- Group II: clear arc pattern (non-linearity)
- Group III: one large outlier residual
- Group IV: all residuals near zero except the single leverage point

### 6. One-Way ANOVA
Compares y means across all four groups. Result: **F ≈ 0, p ≈ 1.0** — no significant difference detected, because the means are identical by construction.

---

## Key Takeaway

> ANOVA and all summary statistics say the four groups are equivalent. The plots say they are not. **Always visualize your data before trusting summary statistics.**

---

## Requirements

```
pandas
numpy
scipy
matplotlib
jupyter
```

Install with:

```bash
pip install pandas numpy scipy matplotlib jupyter
```

Run the notebook:

```bash
jupyter notebook anscombe_analysis.ipynb
```
