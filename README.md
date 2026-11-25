OTTO – Multi-Objective Recommender System
A session-based recommendation system designed to predict users' next interactions on the OTTO e-commerce platform.

The project implements a multi-objective recommendation pipeline that generates personalized Top-20 recommendations for three interaction types: clicks, carts, and orders.

The system is based on a co-visitation matrix approach, combining historical session interactions, item-to-item relationships, session recency, and fallback recommendations.

The main objective is to build an efficient and scalable recommendation pipeline while managing the memory constraints associated with large-scale e-commerce datasets.
Project Overview
In the OTTO Recommender System challenge, the goal is to predict the items a user is likely to interact with next, based on their recent session activity.

Each session can contain different types of interactions:
- Clicks: Items viewed by the user.
- Carts: Items added to the shopping cart.
- Orders: Items purchased by the user.
The system generates up to 20 recommended items for each session and interaction type.

The final predictions are formatted according to the Kaggle competition requirements.
Architecture
The recommendation pipeline consists of four main stages.
1. Data Loading
The training and test datasets are stored in Parquet format and loaded from Google Drive.

Dataset location:
/content/drive/MyDrive/otto_project/

Input files:
File	Description
train.parquet	Historical user interaction data
test.parquet	Sessions used to generate recommendations


The Parquet format allows efficient storage and processing of large datasets.
2. Co-visitation Matrix Construction
The recommendation system relies on item co-visitation statistics.

A co-visitation matrix captures relationships between items that frequently appear together within user sessions.

The matrix is constructed using the custom function:
build_covis_streamed()


A streaming-based approach is used to process the data progressively and reduce memory consumption.

This strategy is particularly important when working with large datasets that cannot be comfortably loaded and processed entirely in memory.

The generated matrices are saved as reusable artifacts:
Artifact	Description
covis_val.pkl	Co-visitation matrix used for local validation
covis_full.pkl	Co-visitation matrix computed using the full training dataset


The full matrix required approximately 1 hour and 34 minutes to compute.
3. Recommendation Generation
For each session, the system combines several sources of information:
- Recent items from the user's session history.
- Items frequently co-visited with historical items.
- Session recency to prioritize recent interactions.
- Fallback recommendations when the available candidates are insufficient.
The candidate generation process produces recommendations for the three prediction targets:
clicks
carts
orders

Each session and interaction type receives a list of up to 20 recommended item IDs (aid).
4. Submission and Evaluation
The generated recommendations are exported into the final submission file:
submission.csv

The file contains the predictions for all test sessions and interaction types, following the required Kaggle submission format.

The submission contains:
- 5,015,410 rows, including the header.
- Approximately 783 MB of data.
The final predictions are evaluated using the competition's weighted Recall@20 metric.
Results
The system was evaluated using both local validation and the Kaggle leaderboard.
Local Validation
Metric	Recall@20
Clicks	0.46207
Carts	0.47306
Orders	0.74444
Weighted Recall@20	0.63479


The local validation results show that the system performs particularly well on purchase-related interactions, while clicks and cart additions remain more challenging prediction tasks.
Kaggle Evaluation
Evaluation	Score
Public Leaderboard	0.55494
Private Leaderboard	0.55489


The final submission achieved a Public Score of 0.55494 and a Private Score of 0.55489.

The very small difference between the public and private scores indicates consistent performance across the two evaluation subsets.

The submission was recorded on Kaggle with the status:

Complete (after deadline)
Visual Results
Pipeline Artifacts on Google Drive
The following image illustrates the organization of the generated artifacts and datasets on Google Drive.

Kaggle Submission Results
The following screenshot presents the final evaluation results obtained on Kaggle.

Technologies Used
Technology	Purpose
Python	Main programming language
Pandas	Data manipulation
NumPy	Numerical operations
PyArrow / Parquet	Efficient data storage and loading
Google Colab	Development and execution environment
Google Drive	Dataset and artifact storage
Kaggle	Competition evaluation


Limitations and Future Improvements
Although the co-visitation baseline achieved competitive results, several improvements could further enhance recommendation quality.
Current Limitations
- The system relies primarily on statistical co-visitation relationships.
- No supervised reranking model is currently implemented.
- The recommendation strategy uses a relatively simple combination of historical items, co-visitation candidates, and fallback recommendations.
- The temporal weighting strategy could be refined to better capture changes in user interests.
Future Improvements
Possible extensions include:
- Supervised Reranking: Introduce LightGBM or CatBoost to rank candidate items using session-level features.
- Action-Specific Co-visitation: Build separate co-visitation matrices for clicks, carts, and orders.
- Temporal Weighting: Apply more sophisticated time-decay strategies to prioritize recent interactions.
- Feature Engineering: Incorporate additional behavioral features, such as interaction frequency and session patterns.
- Candidate Optimization: Improve candidate generation to increase recommendation diversity and relevance.
These improvements could help bridge the gap between the local validation score and the final Kaggle leaderboard performance.
Key Takeaways
This project provided practical experience in:
- Building a session-based recommendation system.
- Working with large-scale e-commerce interaction data.
- Implementing memory-efficient data processing.
- Constructing item co-visitation matrices.
- Designing a multi-objective recommendation pipeline.
- Generating large-scale Top-20 predictions.
- Evaluating recommendation quality using Recall@20.
- Preparing and submitting predictions to Kaggle.
The project demonstrates how a statistical recommendation baseline can be developed into a complete and scalable pipeline, from raw interaction data to final competition submission.
Repository
GitHub Repository:
otto-recommender-system

Competition:
OTTO – Multi-Objective Recommender System
Note: The datasets and generated artifacts are stored separately on Google Drive due to their large size and are not included directly in the GitHub repository.
