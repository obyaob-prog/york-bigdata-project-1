# What Drives Baseball Team Success? (1960–2010)

**York University, School of Continuing Studies — Certificate in Big Data Analytics, Course 2 (Fall 2024), Group 4.**
Rebuilt & verified September 2026 from the original October 2024 notebook.

## Business question
A baseball front office wants to know which team-level statistics actually move the win column — scoring more runs (offense), preventing runs (defense/pitching), or the balance of the two — so it can decide where to invest.

## Data
`data/baseball_teams.csv` — one row per team per season (Lahman-style), 43 columns, 2,805 team-seasons (1871–2015).
Analysis uses the **1960–2010 window (1,312 team-seasons)**: pre-1960 seasons were as short as 6 games, so mixing eras would contaminate every distribution. No missing values in any column used.

## Method (why each step)
1. **Descriptives + skewness** — check center/scale/symmetry before trusting any correlation.
2. **Histograms + boxplots** — expose outlier seasons (e.g. strike-shortened 1981/1994/1972).
3. **Pearson correlations on six targeted pairs** — each maps to one business question (offense→wins, defense→wins, balance→wins, plus loss-side mirrors).
4. **Scatterplot (wins vs run differential)** — confirm the headline correlation is genuinely linear, not driven by extremes.
5. Derived variable **Run_Differential = Runs_Scored − Runs_Against** — captures the offense/defense balance in one number.

## Results
| Pair | r |
|---|---|
| Wins vs Run Differential | **+0.85** |
| Wins vs Runs Scored | +0.60 |
| Wins vs Runs Against | −0.25 |
| Losses vs Run Differential | −0.85 |

**Run differential dominates**: |r| ≈ 0.85 vs 0.60 (offense alone) and 0.25 (defense alone). A run prevented is worth about as much as a run scored (win/loss correlations are near mirror images). Caveat: the differential is close to a bookkeeping summary of winning — best in-sample descriptor, not an independently pullable causal lever.

## Recommendation
Judge roster moves by **net expected runs** (added minus prevented). If forced to pick one lever, pick offense — but expect diminishing returns without competent run prevention. Evaluate pitchers on ERA, not raw runs allowed.

## Files
- `project1-group4-clean.ipynb` — full reproducible analysis (executed, outputs embedded; runs from any launch folder, no path editing)
- `REPORT.md` — full report · `VERIFICATION.md` — audit trail of what was checked and fixed
- `data/baseball_teams.csv` — dataset

## Run it
Any Python 3.11 env with `jupyterlab`, `pandas`, `matplotlib`, `seaborn` (see `environment.yml` in the release bundle).
