OTTO – Multi-Objective Recommender System
A session-based recommendation system developed for the OTTO Kaggle competition. The objective is to predict the next items a user is likely to interact with across three interaction types: clicks, carts, and orders.

The system combines item co-visitation, session history, recency, and fallback recommendations to generate Top-20 predictions.
Architecture
The pipeline is organized into four main stages:
       ┌──────────────────────────────┐
       │       Input Data             │
       │                              │
       │  train.parquet               │
       │  test.parquet                │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │   Co-visitation Matrices     │
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
       │   Candidate Generation       │
       │                              │
       │   • Recent session items     │
       │   • Co-visitation neighbors  │
       │   • Session recency          │
       │   • Fallback recommendations│
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │   Top-20 Predictions         │
       │                              │
       │   • Clicks                   │
       │   • Carts                    │
       │   • Orders                   │
       └──────────────┬───────────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │   Submission & Evaluation   │
       │                              │
       │   submission.csv             │
       │   Kaggle Evaluation          │
       └──────────────────────────────┘

Generated Artifacts
Artifact	Description	Size
covis_val.pkl	Validation co-visitation matrix	186 MB
covis_full.pkl	Full training co-visitation matrix	187 MB
submission.csv	Final Kaggle predictions	783 MB


The full co-visitation matrix required approximately 1h34 of computation.
Results
Local Validation
Evaluation using Weighted Recall@20:
Target	Recall@20
Clicks	0.46207
Carts	0.47306
Orders	0.74444
Overall	0.63479


Kaggle Evaluation
Metric	Score
Public Score	0.55494
Private Score	0.55489


The small difference between public and private scores shows consistent performance across both evaluation sets.
Limitations & Future Improvements
- Implement supervised reranking using LightGBM or CatBoost.
- Build action-specific co-visitation matrices.
- Explore more advanced temporal weighting strategies.
- Improve candidate generation and recommendation relevance.
Technologies
Python · Pandas · NumPy · PyArrow · Google Colab · Google Drive · Kaggle
Repository
GitHub – OTTO Multi-Objective Recommender System

OTTO Kaggle Competition
