# OTTO – Multi-Objective Recommender System

Pipeline de recommandation multi-objectif (prédiction des `clicks`, `carts` et `orders` par session) basé sur une matrice de co-visitation calculée en streaming (`build_covis_streamed`), combinée à la récence de session et à un repli (*fallback*).

## Architecture du pipeline

1. **Entrées (`otto_parquet/`)** : chargement des données d'entraînement (`train.parquet`, 2.1G) et de test (`test.parquet`, 74M) depuis Google Drive (`/content/drive/MyDrive/otto_project/`).
2. **Construction des matrices de co-visitation (`artifacts/`)** :
   - Calcul en streaming via `build_covis_streamed` pour éviter la saturation mémoire.
   - Sauvegarde des artefacts : `covis_val.pkl` (186M) et `covis_full.pkl` (187M, ~1h34 de calcul).
3. **Génération des prédictions (Top-20)** : combinaison de l'historique récent de la session, des voisins de co-visitation et d'un *fallback* pour produire 20 `aid` par `session_type` (`_clicks`, `_carts`, `_orders`).
4. **Export & Évaluation** : génération de `submission.csv` (783M, 5 015 410 lignes incluant l'en-tête) et évaluation sur Kaggle.

![Organisation des artefacts sur Google Drive](assets/drive_artifacts.png)

## Résultats

| Évaluation | Score global / métrique | Détails |
| :--- | :--- | :--- |
| **Validation locale** | **Weighted Recall@20 : `0.63479`** | `clicks = 0.46207`, `carts = 0.47306`, `orders = 0.74444` |
| **Kaggle Public Score** | **`0.55494`** | Soumission `submission.csv` (`Complete (after deadline)`) |
| **Kaggle Private Score** | **`0.55489`** | Écart très faible avec le score public |

![Résultats de soumission sur Kaggle](assets/kaggle_submission_score.png)

## Limites et pistes d'amélioration

- Modèle baseline de co-visitation sans étape de *reranking* supervisé (ex. LightGBM / CatBoost).
- Matrices distinctes par type d'action (`clicks`, `carts`, `orders`) et pondération temporelle plus fine à explorer.
