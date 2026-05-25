# Centipede Game Defection Rate Heatmaps

R notebook for visualizing defection patterns in finite and infinite N-player centipede games. Generates heatmaps across rounds and repetitions using two defection rate calculation approaches.

---

## Notebook structure

The notebook has three self-contained analysis sections. Each section can be run independently by running its setup cell first.

**Setup cells**
- `# OLD data setup` -> loads the finite game CSVs; filters treatments and computes node stats
- `# NEW data setup` -> loads the infinite game RDS

**Analysis sections**

| Section | Data | Approach | Outputs |
|---|---|---|---|
| 1 | Finite (8P, 2P8R) | CDF | `heatmap_8p_cdf.png`, `heatmap_2p8r_cdf.png` |
| 2 | Finite (8P, 2P8R) | Conditional defection rate | `heatmap_8p_cond.png`, `heatmap_2p8r_cond.png` |
| 3 | Infinite | Conditional defection rate | `heatmap_inf_cond.png` |

---

## Defection rate approaches

### CDF approach
The proportion of all games in a repetition that ended **at or before** round N. Non-decreasing across rounds as earlier defections accumulate. 

(consistent with fig. 6 in original paper)

### Conditional defection rate approach
Among games that reached round N, the proportion that defected there. Each round is evaluated independently of earlier ones. 

(consistent with bar chart calculations in NEW data analysis of infinite game)

### Why no CDF for the infinite game?
The CDF requires knowing where each complete game pathway ended (group-level data). The infinite game used a between-subjects design: each participant was assigned a single player level and made one decision, with no identifier linking players into sequential game pathways. Without a group-level endpoint, the CDF cannot be computed directly. One possible workaround would be to construct synthetic game pathways via bootstrapping (repeatedly sampling one player per level, defining the endpoint as the first defection).

---

## Data

Data files are not included. Contact to request.

- **Finite** (Sections 1 & 2): `data/data_paper_player.csv`, `data/data_paper_group.csv`
- **Infinite** (Section 3): `data/NEWprocessed.rds`

---

## Requirements

```r
install.packages(c("tidyverse", "patchwork", "RColorBrewer"))
```
