# Centipede Game Defection Rate Heatmaps

R notebook for visualizing defection patterns in finite and infinite N-player centipede games. Generates heatmaps across rounds and repetitions using two defection rate calculation approaches.

---

## Notebook structure
 
Four self-contained sections grouped under two games. Each section can be run independently by running its setup cell first.
 
### Finite N-Player Centipede Game (8P, 2P8R)
*Setup cell:* `# OLD data setup` -> loads `OLDdata_paper_player.csv`, `OLDdata_paper_group.csv`
 
| Section | Approach | Outputs |
|---|---|---|
| 1 | CDF | `heatmap_8p_cdf.png`, `heatmap_2p8r_cdf.png` |
| 2 | Conditional defection rate | `heatmap_8p_cond.png`, `heatmap_2p8r_cond.png` |
 
### Infinite N-Player Centipede Game
*Setup cell A:* `# NEW data setup` -> loads `NEWprocessed.rds` (individual-level, between-subjects)  
*Setup cell B:* `# games.csv data setup` -> loads `games.csv` (game-level, with stop reason)
 
| Section | Approach | Outputs |
|---|---|---|
| 3 | Conditional defection rate | `heatmap_inf_cond.png` |
| 4.a | CDF, all stopping events (player defection + computer termination) | `heatmap_inf_cdf_dc.png` |
| 4.b | CDF, player defection only (KM-adjusted denominator) | `heatmap_inf_cdf_d.png` |
 
---
 
## Defection rate approaches
 
### CDF approach (consistent with fig. 6 from original paper)
The proportion of all games that ended **at or before** round N. Non-decreasing across rounds as earlier stopping events accumulate. 

In the **finite games**, games only end by player choice (or reaching final round), so the denominator is simply the total number of games in that repetition, fixed throughout all rounds.

For the **infinite game**, two variants are computed from `games.csv`, which contains game-level records with a `stop_reason` field (`"player"` or `"computer"`):

- **All stopping events (4.a):** counts both player defections and computer terminations toward the cumulative total. Denominator is total number of games, fixed at N=987.

- **Player defection only (4.b):** counts only rounds where the player chose to defect. Uses a Kaplan-Meier (KM) style adjusted denominator to handle computer terminations as right-censored observations. At each round, games terminated by the computer in *prior* rounds are removed from the risk set; games that ended by player defection remain in the denominator throughout, mirroring the finite game CDF logic. This is the most appropriate comparison to the finite game CDF, as both estimate the same estimand: the cumulative probability of player-initiated defection by round N.
   - **Independent and Non-Informative Censoring Assumption for KM Estimator:** Computer termination at each round is assigned with fixed 1/3 probability independently of the sampled player decisions. Because pathways are reconstructed from independent between-subjects observations rather than real sequential games, later-round risk sets are not behaviorally selected for cooperators, and the non-informative censoring assumption holds. The KM estimator therefore recovers an unbiased estimate of cumulative defection probability under the empirical between-subjects distribution behavior.

### Conditional defection rate approach (consistent with bar chart calculations from inf game analysis)
Among games that reached round N, the proportion that defected there. Each round is evaluated independently of earlier ones, games that stopped before round N are excluded from both numerator and denominator. 

Note: the infinite game conditional defection rate (Section 3) uses `NEWprocessed.rds`, which is individual-level between-subjects data: each participant made one decision at one assigned level. The CDF sections (4 & 5) require `games.csv`, which has reconstructed synthetic game-level pathways needed to define cumulative stopping points.
 
---

## Data

Data files are not included. Contact to request.

- **Finite** (Sections 1 & 2): `data/data_paper_player.csv`, `data/data_paper_group.csv`
- **Infinite** (Section 3, 4, & 5): `data/NEWprocessed.rds`, `data/games.csv`


---

## Requirements

```r
install.packages(c("tidyverse", "patchwork", "RColorBrewer"))
```
