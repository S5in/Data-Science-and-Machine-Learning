# Project 4: Film Recommender

**Question:** Can a recommender system learn a person's taste from their film ratings and suggest films they will like? Which method works best?

**Data:** [MovieLens ml-latest-small](https://grouplens.org/datasets/movielens/) (GroupLens Research): 100,836 ratings of 9,742 films by 610 users, 1996–2018, in [`data/ml-latest-small.zip`](data/). My own ratings of 60 films are in [`data/my_ratings.csv`](data/my_ratings.csv).

**Notebook:** [`film_recommender.ipynb`](film_recommender.ipynb) (HTML copy: `film_recommender.html`).

## Methods

1. **Most popular** films (baseline)
2. **Content-based:** TF-IDF of genres and user tags, compared with cosine similarity
3. **Item-based collaborative filtering:** adjusted-cosine similarity between films
4. **Matrix factorisation for rating prediction:** biased MF trained with stochastic gradient descent, written from scratch in NumPy
5. **PureSVD:** truncated SVD of the sparse ratings matrix, for ranking

## Results

Each user's ratings were split by time (60% train, 20% validation, 20% test). Settings were chosen on validation, and the test set was used once.

| Method | Precision@10 | Recall@10 | NDCG@10 | Hit rate | Coverage |
|---|---|---|---|---|---|
| PureSVD (20 factors) | 0.085 | **0.086** | **0.107** | **46.6%** | 5.1% |
| Item-based CF (k=50) | **0.086** | 0.068 | **0.107** | 43.1% | **7.1%** |
| Most popular | 0.058 | 0.050 | 0.076 | 32.3% | 1.0% |
| Content-based | 0.031 | 0.024 | 0.035 | 22.0% | 4.1% |
| Rating-prediction MF | 0.031 | 0.023 | 0.034 | 21.6% | 2.7% |

- **PureSVD and item-based CF are essentially tied** and both beat the popularity baseline. PureSVD finds more liked films, and item-based CF recommends more varied, less popular ones.
- **Predicting ratings is not the same as recommending.** The SGD model has a test RMSE of 0.889 stars, against 1.068 for guessing the average, yet it's one of the weakest recommenders.
- **The ratings matrix is 98.3% empty**, so it's stored as a sparse matrix: 1.2 MB instead of 47.5 MB.

## My recommendations

51 of my 60 films matched MovieLens. The other 9 were released after 2018. Item-based CF and PureSVD recommend films such as *Reservoir Dogs*, *Pulp Fiction*, *The Usual Suspects*, *American History X* and *The Departed*. Item-based CF also explains each recommendation, for example "*Reservoir Dogs*, because I liked *Goodfellas*, *The Godfather Part II* and *Fight Club*".

## Contents (CRISP-DM)

1. Business understanding
2. Setup
3. Data infrastructure: MovieLens tables and the sparse ratings matrix
4. Data understanding
5. Data quality summary
6. Provenance, licence and ethics
7. Data preparation: per-user time split
8. Recommendation methods, tuned on validation
9. Final test
10. My recommendations
11. Deployment
12. Conclusion
