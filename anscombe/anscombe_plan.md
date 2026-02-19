## Analysis of Anscome Data


## Goal
- Analyze the data set groups using approprate methods suchs as finding means and  standard errors and standard errors of means and keep them seperate until comparing across groups when ANOVA and see what else we want to do with it

## Immplementation
- We are gonna look at the data and figure out what visual will work best
- Save these proposals
- i will approve it before continunng so them to the end of this md

---

## Iteration 1 — Results & Visualization Proposals

### Descriptive Statistics (per group, computed from anscombe_quartet.tsv)

| group | var | n  | mean   | SD     | SEM    |
|-------|-----|----|--------|--------|--------|
| I     | x   | 11 | 9.0000 | 3.3166 | 1.0000 |
| I     | y   | 11 | 7.5009 | 2.0316 | 0.6125 |
| II    | x   | 11 | 9.0000 | 3.3166 | 1.0000 |
| II    | y   | 11 | 7.5009 | 2.0317 | 0.6126 |
| III   | x   | 11 | 9.0000 | 3.3166 | 1.0000 |
| III   | y   | 11 | 7.5000 | 2.0304 | 0.6122 |
| IV    | x   | 11 | 9.0000 | 3.3166 | 1.0000 |
| IV    | y   | 11 | 7.5009 | 2.0306 | 0.6122 |

### Key Observation
All four groups share nearly identical summary statistics (mean x = 9.0, mean y ≈ 7.5,
SD and SEM also nearly equal across groups). Summary stats alone would suggest the
groups are interchangeable — yet their underlying structures are completely different.

### Proposed Visualizations
1. **2×2 scatter-plot grid** with OLS regression line per group (`anscombe_scatter.png`)
   — reveals actual geometric structure (linear, curved, outlier-driven)
2. **Mean ± SEM bar chart** for y by group (`anscombe_means.png`)
   — deliberately "wrong" visualization to illustrate what gets hidden
3. **Residual plots** y − ŷ per group (`anscombe_residuals.png`)
   — exposes non-linearity (II), influential outliers (III, IV)
4. **One-way ANOVA** comparing y across groups — expected p > 0.05 by design

---

## Iteration 2 — ANOVA + Visualizations

### ANOVA Results
One-way ANOVA on y across groups I–IV:
- **F = 0.0000, p = 1.0000** → No significant difference in group means (p > 0.05)
- This is expected: all four groups were constructed to have the same y mean (~7.5)
- ANOVA alone would lead you to conclude the groups are equivalent — they are not

### Visualizations Produced
| File | Description |
|------|-------------|
| `anscombe_scatter.png` | 2×2 scatter grid + OLS line; reveals linear, curved, outlier-driven, vertical-cluster patterns |
| `anscombe_means.png` | Mean ± SEM bar chart with ANOVA annotation; shows how bar charts hide structure |
| `anscombe_residuals.png` | Residual plots per group; exposes curvature (II), high-leverage point (III), single-point drive (IV) |

### Conclusions
- **Group I**: Classic linear relationship with normal scatter around the regression line
- **Group II**: Clearly non-linear (quadratic); linear model is a poor fit
- **Group III**: Linear with one high-leverage outlier pulling the regression line
- **Group IV**: All x values clustered at 8 except one outlier at x=19; regression entirely determined by that single point
- **Takeaway**: ANOVA p > 0.05 and identical summary statistics across all four — yet the data are structurally incomparable. Always visualize before trusting summary statistics.

---

## Addendum — Regression Analysis & Notebook

### Changes
- PNGs removed; all outputs now live inline in `anscombe_analysis.ipynb`
- Added full regression table per group (slope, SE(slope), intercept, SE(intercept), R, R², p)
- Scatter plots updated with 95 % confidence band and per-panel annotation box
- Residuals now derived from `scipy.stats.linregress` for consistency with regression table

### Regression Results (all groups identical by design)
| group | slope  | intercept | R²     | p(slope) |
|-------|--------|-----------|--------|----------|
| I     | 0.5001 | 3.0001    | 0.6665 | 0.0022   |
| II    | 0.5000 | 3.0009    | 0.6662 | 0.0022   |
| III   | 0.4997 | 3.0025    | 0.6663 | 0.0022   |
| IV    | 0.4999 | 3.0017    | 0.6667 | 0.0022   |
