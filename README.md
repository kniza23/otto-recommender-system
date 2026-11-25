# OTTO – Multi-Objective Recommender System

A session-based recommendation system developed for the OTTO Kaggle competition. The objective is to predict the next items a user is likely to interact with across three interaction types: **clicks**, **carts**, and **orders**.

The system combines item co-visitation, session history, recency, and fallback recommendations to generate Top-20 predictions.

---

## Architecture

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
