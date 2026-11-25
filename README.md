# OTTO – Multi-Objective Recommender System

A session-based recommendation system developed for the **OTTO – Multi-Objective Recommender System** Kaggle competition. The objective is to predict the next items (`aid`) a user is likely to interact with across three interaction types: **clicks**, **carts**, and **orders**.

The system combines **streaming item co-visitation**, **session history**, **recency weighting**, and **popularity fallback** to generate Top-20 predictions per session and action type.

---

## Problem Formulation & Metric

In e-commerce sessions, users browse (`clicks`), add items to their cart (`carts`), and purchase (`orders`). Given the truncated history of a test session, the model outputs **20 recommended item IDs (`aid`)** for each target (`session_clicks`, `session_carts`, `session_orders`).

Performance is evaluated using **Weighted Recall@20**:

$$\text{Score} = 0.10 \cdot \text{Recall@20}_{\text{clicks}} + 0.30 \cdot \text{Recall@20}_{\text{carts}} + 0.60 \cdot \text{Recall@20}_{\text{orders}}$$

---

## Architecture & Methodology

The pipeline is organized into four main stages leading to submission and evaluation:

```text
       ┌──────────────────────────────┐
       │         Input Data           │
       │                              │
       │  train.parquet               │
       │  test.parquet                │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │     Co-visitation Matrices   │
       │                              │
       │   build_covis_streamed()     │
       │                              │
       │   • Streaming computation    │
       │   • Memory optimization      │
       │   • Artifact generation      │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │     Candidate Generation     │
       │                              │
       │   • Recent session items     │
       │   • Co-visitation neighbors  │
       │   • Session recency          │
       │   • Fallback recommendations │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │       Top-20 Predictions     │
       │                              │
       │   • Clicks                   │
       │   • Carts                    │
       │   • Orders                   │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │   Submission & Evaluation    │
       │                              │
       │   submission.csv             │
       │   Kaggle Evaluation          │
       └──────────────────────────────┘
````


---

## Results

### Local Validation

Evaluated on the local validation split using **Weighted Recall@20**:

| Target | Weight | Recall@20 |
| :--- | :---: | :---: |
| Clicks | `0.10` | `0.46207` |
| Carts | `0.30` | `0.47306` |
| Orders | `0.60` | `0.74444` |
| **Weighted Overall** | **`1.00`** | **`0.63479`** |

### Kaggle Leaderboard Evaluation

| Metric | Score | Notes |
| :--- | :---: | :--- |
| **Public Score** | **`0.55494`** | Late submission (`submission.csv` — `Complete (after deadline)`) |
| **Private Score** | **`0.55489`** | High consistency with Public Score (`Δ = 0.00005`) |

### Key Takeaways from Results

- **High Orders Recall (`0.74444`)**: Combining session recency (reverse chronological items) with co-visitation candidates captures repeat views and cart-to-order conversions effectively.
- **Public vs. Private Stability**: A difference of only `0.00005` between the Public Score (`0.55494`) and Private Score (`0.55489`) indicates reliable generalization without overfitting.
- **Local vs. Kaggle Gap (`0.63479` vs. `~0.555`)**: Expected on OTTO due to test session truncation points, distribution shifts, and the use of a single co-visitation baseline without a supervised second-stage reranker.


---

## Technologies & Data

- **Languages & Libraries**: `Python`, `Pandas`, `NumPy`, `PyArrow`
- **Data & Artifacts**: `Parquet` (`train.parquet`, `test.parquet`), `Pickle` (`covis_val.pkl`, `covis_full.pkl`), `CSV` (`submission.csv`)
- **Platforms & Tools**: `Google Colab`, `Google Drive`, `Kaggle`, `GitHub`


## Competition & Links

- **Kaggle Competition**: [OTTO – Multi-Objective Recommender System](https://www.kaggle.com/c/otto-recommender-system)
- **Kaggle Profile**: [kenza Bousbaa](https://www.kaggle.com/kenzabousbaa)
