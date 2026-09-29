# Project 1 — Sprint 1: What drives baseball team success? (1960–2010)

**York University, School of Continuing Studies — Certificate in Big Data Analytics, Course 2 (Fall 2024)**
**Group 4:** Jinal, Yann, Olga, Reine · Rebuilt & verified September 2026 from the October 2024 notebook

---

## 1. Business question

A baseball front office wants to know **which team-level statistics actually move the win column**,
so it can decide where to invest: scoring more runs (offense), preventing runs (defense/pitching),
or both. The analysis answers four questions:

1. What offensive traits are common among consistently high-performing teams?
2. How do defensive metrics (runs allowed, ERA, walks allowed, pitcher strikeouts) influence team success?
3. Can run-based indicators predict a team's win total?
4. Where should a team focus improvement effort — scoring, run prevention, or the balance of the two?

## 2. Data

`baseball_teams.csv` — one row per team per season (Lahman-style team table), 43 columns,
2,805 team-seasons spanning **1871–2015**. No missing values in any column used.

**Key data-quality decision:** before 1960, seasons were as short as 6 games; the modern era plays
a stable ~162-game schedule. Mixing eras would contaminate every distribution with incomparable
season totals, so **all analysis uses the 1960–2010 window: 1,312 team-seasons** — the window the
business question asks about.

**Variables (why each):** `Games_Won`, `Games_Lost` (outcomes) · `Runs_Scored`, `Hits` (offense) ·
`Runs_Against`, `Earned_Run_Average` (pitching quality, normalized per 9 innings),
`Walks_Allowed`, `Strikeouts_Allowed` (defense/pitching). Note: despite its name,
`Strikeouts_Allowed` counts strikeouts **recorded by** the team's pitchers — higher means better
pitching (confirmed: r = +0.35 with wins).

## 3. Method (each step justified)

1. **Descriptive statistics + skewness** — establish center, scale and symmetry before trusting any
   correlation; a long tail or broken scale would invalidate "average team" reasoning.
2. **Histograms (20 bins) + boxplots** — visualize shape and expose outlier seasons; same bins
   across variables keep the comparison honest.
3. **Pearson correlations on six targeted pairs** — for continuous season totals, Pearson's r is
   the direct measure of linear association, and each pair maps to one business question
   (offense→wins, defense→wins, balance→wins, and the loss-side mirrors).
4. **One scatterplot (wins vs run differential)** — checks the headline r = 0.85 is a genuine
   linear relationship, not an artifact of a few extreme seasons.

A derived variable, **Run_Differential = Runs_Scored − Runs_Against**, captures the
offense/defense *balance* in a single number instead of leaving it implied.

## 4. Results

**Distributions (1960–2010, corrected):** six of eight variables are approximately symmetric
(|skew| ≤ 0.32). ERA is slightly right-skewed (+0.30; a few disastrous pitching seasons). Hits is
left-skewed (−1.43) — the left tail is fully explained by **strike-shortened seasons
(1981, 1994, 1972)**, when teams played ~103–117 games and every counting stat collapsed.
`Strikeouts_Allowed` has a wide, flat-topped spread (std ≈ 156), reflecting how pitching changed
across the 50-year window — it is not uniform.

**Correlations (all re-verified by execution):**

| Pair | r | Reading |
|---|---|---|
| Games Won vs Runs Scored | **+0.60** | Moderate: scoring more goes with winning more |
| Games Won vs Runs Against | **−0.25** | Weak: run prevention alone explains little of wins |
| Games Won vs Run Differential | **+0.85** | Strong: balance dominates |
| Games Lost vs Runs Scored | **−0.23** | Weak mirror of the offense effect |
| Games Lost vs Runs Against | **+0.61** | Moderate: leaky defenses lose games |
| Games Lost vs Run Differential | **−0.85** | Strong mirror: outscoring opponents avoids losses |

Supporting: ERA correlates −0.44 with wins (the cleaner pitching metric); pitcher strikeouts
+0.35 with wins; walks allowed −0.16 with wins (weak).

## 5. Interpretation

**The run differential is the story.** At |r| ≈ 0.85 it explains team success far better than
offense alone (0.60) or defense alone (−0.25). The honest reading is symmetric: **a run prevented
is worth about as much as a run scored** (win/loss correlations with the differential are near
mirror images, +0.85 / −0.85). Offense contributes more than defense *alone*, but neither side
alone comes close to the balance metric — the original draft's "offense definitely wins" claim
overreached and is corrected here.

**Caveat:** the differential is close to a bookkeeping summary of winning — every game is decided
by who outscores whom, so a season differential *aggregates* game outcomes almost by construction.
It is the best in-sample descriptor in this data, not an independently pullable causal lever.

## 6. Recommendation

1. **Optimize the differential, not one side of it.** Judge roster moves by net expected runs
   (runs added minus runs prevented).
2. **If forced to pick one lever, pick offense** — but expect diminishing returns without
   competent run prevention.
3. **Evaluate pitchers on ERA, not raw runs allowed**; remember the column-name trap
   (pitcher strikeouts help, walks allowed hurt weakly).
4. **Next analytical step (not done here):** the original plan mentioned testing predictive
   models — fit a simple linear regression of wins on run differential as a baseline, add ERA
   and Hits, and validate out-of-sample (train 1960–2000, test 2001–2010) before using it
   for decisions.

---
*Full reproducible workings: `project1-group4-clean.ipynb` (executed, all outputs embedded).
What was checked and what couldn't be: see `VERIFICATION.md`.*
