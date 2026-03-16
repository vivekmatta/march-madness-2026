# March Madness 2026 Bracket Predictor

A single-page bracket prediction app for the 2026 NCAA Tournament. Uses a logistic regression model trained on 10 years of college basketball rating data (2016–2025) to predict game outcomes, then lets you confirm or correct results as the tournament unfolds.

## Quick Start

```bash
cd march-madness-2026
python3 -m http.server 8080
# Open http://localhost:8080/bracket.html
```

> **Important:** Must be served over HTTP — opening `bracket.html` directly as a `file://` URL will block the CSV loading.

## Features

- **ML predictions** — logistic regression (150 iterations) trained on ~900 historical tournament games across 8 rating systems: KenPom/Barttorvik, RPPF, Z-Rating, 538, EvanMiya, Teamsheet (NET/KPI), and Public Picks
- **Full 68-team bracket** — all 4 First Four games wired into the correct R64 slots
- **Real-time corrections** — click "Wrong" on any game, pick the actual winner, and all downstream predictions update automatically
- **Win probability bars** — each game card shows a gold probability bar with percentage
- **Upset detection** — orange/yellow dots highlight predicted upsets
- **Persistent state** — bracket saves to `localStorage` and restores on reload
- **Export** — print/save bracket via browser print dialog

## Data

All CSV files live in `data/` (38 files, 2016–2025 tournament data):

| Category | Files |
|----------|-------|
| Primary ratings | `KenPom Barttorvik.csv`, `RPPF Ratings.csv`, `Z Rating Teams.csv`, `538 Ratings.csv`, `EvanMiya.csv`, `Teamsheet Ranks.csv` |
| Picks & simulation | `Public Picks.csv`, `Tournament Simulation.csv`, `Heat Check Ratings.csv` |
| Historical records | `Seed Results.csv`, `Team Results.csv`, `Coach Results.csv`, `Tournament Matchups.csv` |
| Conference & splits | `Conference Stats.csv`, `Shooting Splits.csv`, `Barttorvik Neutral.csv`, ... |
| Rankings | `TeamRankings.csv`, `AP Poll Data.csv`, `KenPom Preseason.csv`, ... |

## Model Details

Features used (difference between team A and team B, normalized 0–100 per year):
- `ΔBADJ_EM` — Barttorvik adjusted efficiency margin
- `ΔRPPF_RATING` — RPPF composite rating
- `ΔZ_RATING` — Z-Rating
- `ΔPOWER_538` — 538 power rating
- `ΔRELATIVE_EVAN` — EvanMiya relative rating
- `ΔNET_INV` — NET rank (inverted)
- `ΔKPI_INV` — KPI rank (inverted)
- `ΔPUBLIC_R64` — public R64 pick percentage

Win probability: `0.8 × model_sigmoid + 0.2 × seed_history_rate`, clamped to [2%, 98%].

## 2026 Bracket

**First Four** (March 17–18, Dayton):
- FF1: UMBC vs Howard → Midwest 1 vs 16 slot
- FF2: Texas vs NC State → West 6 vs 11 slot
- FF3: Prairie View A&M vs Lehigh → South 1 vs 16 slot
- FF4: Miami OH vs SMU → Midwest 6 vs 11 slot

**Regions:** East (Washington D.C.) · South (Houston) · Midwest (Chicago) · West (San Jose)

**Final Four pairings:** East vs South · Midwest vs West
