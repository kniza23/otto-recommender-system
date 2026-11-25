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
