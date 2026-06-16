# Off-Ball Movement Analysis — Premier League (ML Engineering)

Analytical work delivered for a Premier League first-team environment, covering two
interconnected analyses built on different data modalities: high-frequency optical
tracking and full-season event data. The brief was to produce a rigorous, explainable
metric for a central playmaker's off-ball movement trait and a league-wide similarity
framework to identify comparable profiles for recruitment and tactical planning.

Source code and raw data are proprietary and belong to the club; this repository
documents the methodology, technical design decisions, and example outputs only.

---

## Problem

### Part 1 — Metric Design from Tracking Data

Develop a rigorous, explainable metric for a central playmaker's **off-ball movement**
related to **receiving between the defensive and midfield lines**, and visualize what
makes that movement pattern distinctive across 5 matches of 25 Hz optical tracking data.

The core challenge: positioning alone isn't enough. The player must also be *reachable*
via an unobstructed passing lane. The metric must pass the domain-expert eye test and be
defensible in presentation — not just mathematically correct.

### Part 2 — Player Similarity Framework from Event Data

Using a full-season dataset of off-ball run events (185K events, 379 matches), build a
**movement similarity framework** to identify players with comparable off-ball movement
to the target player. Produce an explainable shortlist, supported by sensitivity checks
and robust QC.

---

## Impact

- Designed and delivered a novel **BtLA (Between-the-Lines Availability)** metric combining
  spatial positioning and continuous passing-lane geometry — derived from first principles,
  fully explainable to coaching and analysis staff.
- Built a **dual-signal player similarity framework** (cosine on movement shape + Euclidean on
  level/volume) across a full season of event data, with structural-null semantics and multiple
  sensitivity checks to ensure shortlist robustness.
- Delivered a full visual output pack: pitch snapshots, spatial density maps, movement trails,
  time-series, comparison views, PCA landscape, radar profiles, and similarity shortlist views.
- Bridged two data paradigms (tracking ↔ event data) by mapping metric components explicitly
  to their event-data proxies, enabling consistent profiling across data sources.

---

## Data (high-level)

| | Part 1 | Part 2 |
|---|---|---|
| **Source type** | Optical tracking (Second Spectrum) | Off-ball run events (Skills Corner) |
| **Granularity** | 25 Hz per-frame (x, y, z per player + ball) | Per-event with 290+ feature columns |
| **Scale** | ~746K frames across 5 matches | ~185K events, 379 matches |
| **Key signals** | Player coordinates, ball position, pass possession label | Run subtype, trajectory, zone, separation, xPass, xThreat |

See [`DATA_DICTIONARY.md`](DATA_DICTIONARY.md) for a detailed schema reference of the event data format.

---

## Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![uv](https://img.shields.io/badge/uv-DE5FE9?style=for-the-badge&logo=astral&logoColor=white)
![Polars](https://img.shields.io/badge/Polars-CD792C?style=for-the-badge&logo=polars&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![msgspec](https://img.shields.io/badge/msgspec-14151A?style=for-the-badge&logo=python&logoColor=white)
![Ruff](https://img.shields.io/badge/Ruff-FCC21B?style=for-the-badge&logo=ruff&logoColor=black)

**Visualization:** `mplsoccer`, `matplotlib`, `seaborn`

---

## Pipeline

### Part 1 — Tracking Data → BtLA Metric

```mermaid
flowchart LR
  A["Raw tracking frames\n(25 Hz JSON)"] --> B["Frame parsing\n(msgspec typed structs)"]
  B --> C["Out-of-play filter\n(ball Z sentinel −10.0)"]
  C --> D["Coordinate normalisation\n(attack direction → +x)"]
  D --> E["Dynamic line detection\n(gap-based heuristic + Savitzky-Golay smooth)"]
  E --> F["BtLA score per frame\n(separation + lane openness sigmoid)"]
  F --> G["Possession filter\n(LASTTOUCH proxy)"]
  G --> H["Per-90 aggregation\n+ comparison across teammates"]
  H --> I["Visuals: snapshots, heatmaps,\ntrails, time-series, scatter decomposition"]
```

### Part 2 — Event Data → Similarity Framework

```mermaid
flowchart LR
  A["Full-season event data\n(185K rows, 290+ cols)"] --> B["Cleaning + structural-null semantics\n(conditional rates with denominator guards)"]
  B --> C["Feature engineering\n(shape vector + level/volume vector)"]
  C --> D["Cosine similarity\n(movement shape)"]
  C --> E["Euclidean distance\n(scaled level / volume)"]
  D --> F["Rank-sum shortlist\n(top-K intersection + fallback)"]
  E --> F
  F --> G["Sensitivity checks\n(thresholds, scaler, mirrored-Y, position filter)"]
  G --> H["QC exports + visual pack\n(PCA, radar, similarity bars, pitch signatures)"]
```

---

## Methodology

### Part 1 — BtLA (Between-the-Lines Availability) Metric

**Core concept.** "Receiving between lines" = positioning in the gap between the
opponent's defensive and midfield lines *and* being reachable via a passing lane.
The BtLA score captures both spatial positioning and playability simultaneously.

**Key design decisions:**

| Decision | Rationale |
|---|---|
| Hard filter on ball Z sentinel (−10.0) | Removes ~39% out-of-play frames (stoppages/dead balls); essential for data hygiene |
| Attack-direction normalisation | Arsenal always attacks toward +x; Period 2 flips automatically — ensures spatial maps are comparable across home/away matches |
| Modal GK exclusion | Excludes opponent goalkeeper (modal identity across match, ~97–99% consistency) from line detection and lane checks — prevents deep CBs from distorting line math |
| Gap-based midfield line heuristic | Sort opponents by depth; deepest 4 = defensive line; next cluster (largest X-gap) = midfield line. K-means rejected: variable cluster sizes caused errors up to 102m |
| Savitzky-Golay smoothing (window=25, polyorder=3) | Segment-aware — applied only within contiguous in-play segments; reduces line noise by ~66% vs raw signal |
| Velocity vector excluded from scoring | 36% of target player's frames show movement toward nearest defender (decoy runs, pressing to receive-and-turn) — penalising this would incorrectly reduce the score by a third; velocity retained for visualisation only |
| Continuous passing lane openness | Minimum defender-to-lane distance mapped through sigmoid; ball height smoothly shrinks effective blocker radius (ball Z increases → harder to intercept → larger effective clearance). No hard thresholds |
| Separation capped at 10m | Prevents extreme open-field frames from dominating; beyond ~10m practical difference for reception feasibility is marginal |
| ≥180 min threshold for comparison | Produces statistically reliable per-90 rates; 11 players qualify (starting XI equivalent) |

**Metric decomposition (why two scatter plots matter):**

The bar chart on `btla_mean` is a blended quantity — high ranks can be driven by
*usage* or *quality*, not necessarily both. The 2×2 faceted scatters split the story:

- **Usage volume**: total minutes between lines (who accumulates the most between-lines availability)
- **Rate**: minutes between lines per 90 (who does it most often once normalised for playing time)
- **Quality (lane openness)**: mean lane openness conditional on being between lines
- **Quality (conditional BtLA)**: mean BtLA score conditional on being between lines

This makes the target player's profile interpretable even if they are not rank #1 on the blended mean.

**Validation (eye test protocol):**

Selected top-5 highest-BtLA frames per match (5 matches = 25 validation panels) with
quality gates: meaningful line gap (≥5m), realistic passing context (teammate within 3m
of ball), 11v11 present, Arsenal possession, temporal diversity (avoid 25Hz plateau
duplicates). Each panel includes match timestamp + frame index for traceability.

---

### Part 2 — Player Similarity Framework

**Core concept.** Convert a season of off-ball run events into a compact, explainable
movement signature per player, then identify the most comparable profiles.

**Two complementary signals:**

| Signal | Feature space | Similarity measure | What it captures |
|---|---|---|---|
| **Shape (cosine)** | Run-type proportions, trajectory direction distribution, zone tendency rates, boolean behaviour rates | Cosine similarity | *How* and *where* a player moves — the distributional fingerprint |
| **Level/volume (Euclidean)** | Spatial means/std, between-lines depth profile, physical/separation metrics, playability proxies, volume (n_events, events_per_match) | Euclidean distance on StandardScaler | *How much, how fast, how deep* — intensity and volume comparability |

**Structural-null semantics.** Several columns are structurally null for players with
certain run subtypes. Instead of imputing or dropping, conditional rates are computed
only when the denominator meets a guard (`DENOM_GUARD=10`), preserving semantic nulls
in exports and imputing at similarity-matrix build time (column-mean imputation +
`np.nan_to_num`).

**Bridge from Part 1.** The Part 2 event-data proxies map directly to Part 1 tracking-data components:

| Part 1 component | Part 2 proxy |
|---|---|
| Between-lines propensity | `DELTA_TO_LAST_DEFENSIVE_LINE_*`, `INSIDE_DEFENSIVE_SHAPE_*` |
| Passing lane playability | `PASSING_OPTION_SCORE`, `XPASS_COMPLETION`, `TARGETED`, `RECEIVED` |
| Separation | `SEPARATION_START/END/GAIN` |
| Spatial signature | Start/end zones, channels/thirds |
| Movement mechanics | `DURATION`, `TRAJECTORY_DIRECTION`, `EVENT_SUBTYPE` |

**Sensitivity checks run:**

- Sample thresholds: ≥50, ≥100 (primary), ≥200 events
- Scalers: `StandardScaler` (primary) vs `RobustScaler`
- Mirrored-Y: side-invariant collapse (left/right symmetry)
- Position filter: restrict to positionally-comparable players
- K−1 subtype basis: drop any one run-type as reference — confirms ranking stability
- Block-weighted cosine: `1/√block_size` weighting across feature blocks
- Shortlist protocols: top-K intersection, rank-sum composite, union fallback

---

## Deliverables

**Part 1:**
- Pitch snapshots with best BtLA frame per match (velocity arrows, line markers)
- Between-lines position density heatmap (volume contours)
- Spatial quality density map (hexbin mean BtLA conditional on between-lines)
- Movement trail plot (top-BtLA windows, segment-safe, time-diverse)
- BtLA time-series (Gaussian-smoothed, 15s + 45s sigma, segment-aware)
- Team comparison bar chart (all-frame + possession-only dual panel)
- 2×2 usage/rate vs quality scatter (lane openness quality)
- 2×2 usage/rate vs quality scatter (conditional BtLA quality)
- 5 per-match validation panels (quality-gated, with traceable timestamps)

**Part 2:**
- PCA landscape of all players (target player highlighted)
- PCA loadings plot (feature interpretation)
- Player similarity bar chart (top-20 context view)
- Radar profile comparison (target vs shortlist players)
- Pitch spatial signatures (start/end zone patterns for shortlist)
- Run subtype stacked bar (movement fingerprint comparison)
- Feature delta heatmap (target vs shortlist player-by-player breakdown)
- QC distribution plots (data quality validation)
- Sensitivity check exports (shortlist stability across parameter choices)

---

## Evidence

### Part 1 — Metric Design

**Pitch snapshots** — best BtLA frame per match with player positions, ball, line markers,
and velocity arrows. Shows the geometric context the metric is measuring.

![Pitch snapshots](assets/part1/pitch_snapshots.png)

**Between-lines volume heatmap** — spatial distribution of where the target player
occupies between-lines positions across all 5 matches (volume only).

![Between-lines heatmap](assets/part1/heatmap_between_lines.png)

**Spatial quality density** — hexbin mean BtLA score conditional on being between lines,
with volume contours overlaid. Shows which zones combine high frequency with high quality.

![Spatial quality density](assets/part1/spatial_density_btla.png)

**Movement trail plot** — top-BtLA windows across all matches, showing the player's
trajectory during highest-quality between-lines availability periods.

![Movement trails](assets/part1/movement_trails.png)

**BtLA time-series** — Gaussian-smoothed BtLA signal across match time (15s and 45s sigma),
showing temporal consistency of the movement pattern.

![BtLA time-series](assets/part1/btla_timeseries.png)

**Team comparison** — dual-panel bar chart comparing all Arsenal outfield players (≥180 min)
on all-frame and possession-only BtLA, contextualizing the target player's profile by role.

![Team comparison bar](assets/part1/btla_comparison_bar.png)

**Usage vs quality decomposition (lane openness)** — 2×2 scatter separating usage volume
(total minutes between lines) and rate (per-90) against conditional lane-openness quality.

![Freq-quality scatter: lane openness](assets/part1/btla_freq_quality_lane_openness.png)

**Usage vs quality decomposition (conditional BtLA)** — same facet structure, but
y-axis shows mean BtLA score conditional on being between lines.

![Freq-quality scatter: conditional BtLA](assets/part1/btla_freq_quality_btla_conditional.png)

---

### Part 2 — Similarity Framework

**PCA landscape** — all players with ≥100 events projected onto the first two principal
components of the shape feature space. Target player highlighted.

![PCA landscape](assets/part2/pca_landscape.png)

**PCA loadings** — feature contributions to the principal components, showing which
movement characteristics drive the similarity space most.

![PCA loadings](assets/part2/pca_loadings.png)

**Similarity bars** — top-20 most similar players by composite rank-sum score (cosine
shape + Euclidean level/volume), providing broader league context.

![Similarity bars](assets/part2/similarity_bars.png)

**Radar profile** — target player vs primary shortlist players across key feature groups
(between-lines depth, playability, separation, run-type mix, zone tendencies).

![Radar](assets/part2/radar.png)

**Pitch spatial signatures** — start/end zone distribution on pitch for target player
and each shortlist player, enabling spatial pattern comparison.

![Pitch signatures](assets/part2/pitch_signatures.png)

**Run-type stacked bar** — movement fingerprint comparison: proportion of each run subtype
for target player vs shortlist.

![Subtype stacked bar](assets/part2/subtype_stacked.png)

**Feature delta heatmap** — per-feature signed delta between target player and each
shortlist player on the scaled level/volume feature space.

![Feature delta heatmap](assets/part2/feature_delta_heatmap.png)

**QC distributions** — data quality validation: distribution checks across key numeric
columns used in similarity scoring, confirming no pathological distributions.

![QC distributions](assets/part2/qc_distributions.png)

---

## Confidentiality

Source code, raw tracking and event data, and all internal identifiers are private and
belong to the club. This repository documents the methodology, validation approach, and
example output artifacts only, without exposing any proprietary assets.
