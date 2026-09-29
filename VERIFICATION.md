# VERIFICATION — Project 1, Group 4 (Sprint 1, Oct 2024)

## Which source version this rebuild corresponds to

- **Primary source:** `work/project 1 -group4/project 1 -group4/Project1- group 4.ipynb` (85 cells).
  Its checkpoint twin (`.ipynb_checkpoints/Project1- group 4-checkpoint.ipynb`) is byte-identical —
  same content, deduped.
- **Earlier draft:** `.ipynb_checkpoints/Project1-checkpoint.ipynb` (73 cells) is a superseded draft:
  same business objective, but distribution/correlation comments are empty "comment:" placeholders
  and the correlation interpretation is a one-liner. Not used.
- **HTML renders:** the 3 files in `raw/group4-project1-html/` (`Project1.html`,
  `Project1 (1).html`, `Project1 (2).html`, dated Oct 19/19/20) are all renders of the **same final
  85-cell notebook** — extracted text differs by only 3–46 characters out of ~289k (render
  metadata), and all three contain the final interpretations. This rebuild corresponds to that
  final version, i.e. effectively **`Project1 (2).html` (Oct 20, latest render)**. No content
  differences between the HTMLs required any merge decision.

## What was run

- Environment: python3, pandas 2.1.4, numpy 1.26.4, matplotlib 3.6.3 (sklearn unused — no
  modeling in this project). No `nbconvert`/`ipykernel` on this machine, so the cleaned notebook
  was assembled by a builder script (`/tmp/p1build/build_notebook.py`, scratch) that executed
  every code cell in order in a shared namespace and embedded stdout + PNG figures as cell
  outputs. All 14 code cells ran without error; all outputs are embedded in
  `project1-group4-clean.ipynb`.
- Data path adapted: notebook reads `data/baseball_teams.csv` (local copy of the original file,
  unchanged, 524,554 bytes).

## Outputs vs original claims

| Original claim | Verified value | Verdict |
|---|---|---|
| GW vs Runs Scored r = 0.5958 | 0.5958 | ✅ confirmed |
| GW vs Runs Against r = −0.2481 | −0.2482 | ✅ confirmed |
| GW vs Scored-Against r = 0.8504 | 0.8504 | ✅ confirmed |
| GL vs Runs Scored r = −0.2268 | −0.2268 | ✅ confirmed |
| GL vs Runs Against r = 0.6111 | 0.6112 | ✅ confirmed |
| GL vs Scored-Against r = −0.8494 | −0.8494 | ✅ confirmed |
| Runs_Against "slightly positively skewed" | skew −0.09 (1960–2010), −0.38 (full) — symmetric/slightly **left** | ❌ wrong direction — corrected |
| Games_Lost "slightly positively skewed" | skew −0.09 / −0.80 — **left** | ❌ wrong direction — corrected |
| Runs_Scored "slightly positively skewed" | skew −0.20 / −0.69 — **left** | ❌ wrong direction — corrected |
| Games_Won "slightly positively skewed" | skew −0.32 / −1.10 — **left** | ❌ wrong direction — corrected |
| Hits "positively skewed" | skew −1.43 / −2.38 — strongly **left** (strike-shortened seasons) | ❌ opposite of truth — corrected |
| Walks_Allowed "positively skewed" | skew −0.07 / −1.37 — **left** | ❌ wrong direction — corrected |
| ERA "slight positive skew" | skew +0.30 / +0.07 — roughly right | ✅ essentially correct |
| Strikeouts_Allowed "nearly uniform" | wide spread (std 156), flat-topped peak ~830–1,040, not uniform | ⚠️ overstated — corrected |
| "Most variables approximately normal" | 6/8 near-symmetric in 1960–2010 window | ✅ fair with corrected directions |
| Final commentary: "team will definitely be more successful with high offenses" | Data: differential 0.85 ≫ offense 0.60 ≫ defense 0.25 | ❌ incoherent with own results — rewritten (balance wins; offense > defense alone, but neither alone ≈ differential) |

## Design changes vs the original (deliberate, documented)

1. **One coherent analysis window (1960–2010) for EDA *and* correlations.** The original plotted
   distributions on the full 1871–2015 data (including 6-game seasons) but correlated on
   1960–2010 — an internal inconsistency. Filtering once, up front, is justified in the notebook.
2. **`Scored-Against` renamed `Run_Differential`** for readability; same definition.
3. **Added one scatterplot** (wins vs differential) to visually validate the headline r = 0.85.
4. **Deleted 2 empty trailing code cells** (dead code). No other dead/abandoned code existed.
5. **Added "why" comments to every code step** per the rebuild rules.
6. **Appendix glossary corrected:** `Strikeouts_Allowed` clarified as strikeouts *by* the team's
   pitchers (higher = better), confirmed by r = +0.35 with wins.
7. **Scope gap noted honestly:** the analytical objective mentioned "determine the accuracy of
   potential predictive models" — no model was ever fitted (EDA + correlations only). The report
   recommends the regression next step instead of pretending it was done.

## What couldn't be verified

- **Subjective distribution prose** (e.g., "most values concentrated around 600–800") — these are
  eyeball readings of plots; the corrected versions were checked against the re-rendered
  histograms, but phrasing remains interpretive.
- **Strike-season explanation for the Hits tail** — verified that all sub-1100-Hit seasons are
  1972/1981/1994 (strike years) with 103–154 games played; the *causal* link (strike → fewer
  games → fewer hits) is domain knowledge, consistent with the data.
- **Original authors' intent** behind the "Year " column's trailing space — preserved as-is;
  filtering code handles it.

## Secrets scan

Grep over `clean/group4-project1-oct/` for `password|token|api_key|secret|credential` (case-insensitive):
no hits. No secrets expected or present in this project.
