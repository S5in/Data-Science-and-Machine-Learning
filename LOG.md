# Work Log

Maksym Redchenko · Data Science & ML 1 CA portfolio. Updated at least weekly.

> Draft entries reconstructed from my notes and commit history. Edit them to match what you actually did each week.

## Week of 5 October 2026
- Read the CA brief and lecture notes (Chapters 1, 3a, 5 and the Data Modelling notes) and the linear regression exemplar notebook.
- Set up the local Git repository and connected it to GitHub (S5in/Data-Science-and-Machine-Learning).
- Explored candidate datasets:
  - Irish Property Price Register: 807,936 sales, 2010–2026. Chosen for Project 1.
  - UFC fighter statistics (4,111 fighters): fighter-level only, lots of missing reach/DOB and placeholder zeros. Rejected.
  - UFC fight-level data from ufcstats.com (11,441 fights, 7 tables). Chosen for Project 2.
- Found a data-leakage problem in the UFC data: fighter stats are career averages as of 2026, so they can't be used to predict past fights.
- Quick test: physical measurements alone predicted the winner only slightly better than always picking the red corner (≈59% vs ≈56%). Decided to engineer pre-fight features (Elo, streaks, rolling stats) instead.
- Wrote and submitted the portfolio specification (drafted with AI assistance, permitted by the lecturer).

## Week of 12 October 2026
- Specification approved by the lecturer.

## Week of 19 October 2026
- Fixed `.gitignore` (leading spaces meant no patterns were being applied).
- Started Project 1 notebook: business understanding, data understanding, cleaning (removed 40,971 non-market sales, 52 duplicates and 5,624 extreme prices; added 13.5% VAT to VAT-exclusive new homes), monthly median series and exploratory analysis.
- Findings so far: national median bottomed out in 2013 and rose 163% by 2026. Dublin is most expensive but grew least since 2013. The new-home premium widened from about 0% to about 50% after 2015.
- Next: time-based train/test split and regression models.

## Week of 26 October 2026
- Reading week. No new code was committed this week; work on the draft resumed on 2–3 November (see next entry).
- Status at the end of the week: Project 1 notebook covered data preparation and exploratory analysis; modelling, evaluation and the conclusion were still to do before the draft deadline (9am, 3 November).

## Week of 2 November 2026
- Completed Project 1 for the draft portfolio submission:
  - Time-based train/test split: trained on 2010–2023, tested on January 2024 to September 2026.
  - Models: naive baseline, linear regression, polynomial regression (degrees 1–7), Ridge and Lasso tuned with time-series cross-validation, and a log-linear growth model on the recovery period (2014+).
  - Result: high-degree polynomials overfit badly. The best models (Lasso degree 2 and the growth model) forecast the national median within about 2% (MAPE).
  - County-level growth models for the six largest counties: growth 5.5% (Dublin) to 10.1% (Limerick) a year. All counties grew slightly more slowly in 2024–2026 than their 2014–2023 trend.
- Finished Project 1, adding the remaining topics from the lectures:
  - Data understanding: compared the CSV, JSON and Parquet versions of the data (99 MB, 224 MB and 24 MB), recorded the licence (CC BY 4.0) and provenance, rated the data on the 5-star open data scale, and added a data quality summary.
  - Ethics and privacy section (addresses dropped after cleaning, aggregated results only).
  - Gaussian basis functions: fit the training data well but can't extrapolate.
  - Multiple linear regression with month-of-year dummies: found a seasonal swing of about 5% and improved the best forecast to 1.8% MAPE. Interpreted the coefficients as % effects.
  - Tested the share of new homes as a feature: it made forecasts worse, so it was left out.
  - Added a Deployment section (CRISP-DM).
- Project 2 (UFC):
  - Built a SQLite database (6 normalised tables, keys and constraints, ETL conversions) and explored the data with SQL.
  - Found and fixed data problems: red corner listed as winner before April 2010 (fighter order randomised, corner never used), 2026 snapshot statistics (rebuilt from earlier fights only), early no-time-limit rules explaining the near-100% finish rate (analysis restricted to 2001+), zero-filled missing round statistics, messy weight classes, an impossible date of birth, and fights from other promotions in the data.
  - Engineered date-strict pre-fight features: record, win rate, streaks, Elo rating (K = 200, chosen on training data), striking, takedown and control rates, and age, height and reach.
  - Logistic regression reached 64.7% accuracy on 2024–2026 fights, against 60.1% for the best simple rule, with well-calibrated probabilities. The leaky snapshot model would have shown a misleading 70.7%.
  - Improved the model, choosing every change on the training years only: a better Elo rating (margin of victory, layoff decay) plus recent form, career length and a long-layoff flag. Test accuracy rose to 65.3% (log loss 0.629, ROC AUC 0.706). Gradient boosting and random forests did no better than logistic regression.
  - When the model is at least 70% confident (about a fifth of fights), it is right 78.9% of the time.
  - Added the Kaggle source and a Deployment section (CRISP-DM). Project 2 complete.
- Next: prepare the week 12 presentation.

## Week of 9 November 2026
- Chose the last two projects: Project 3 (NCT failure prediction) and Project 4 (film recommender, MovieLens).
- Project 3 (NCT):
  - Downloaded the RSA Make and Model Data for all 13 test years (2013–2025) in three formats (PDF, CSV and Excel), plus the 2025 results by test centre.
  - Built a loader that combines all years into one tidy table: header detection for spreadsheets, and regular-expression table extraction for PDFs, including wrapped model names. pdftotext silently dropped 133 rows from 2016, so I switched to pypdf, which recovers every row.
  - Verified the combined data (no missing values, pass + fail = total, no duplicates) and cross-checked 2025 against the centre-level file (98.9% coverage, same fail rate).
  - Applied a consistent privacy threshold (11+ cars) because the RSA's suppression rules changed in 2021 and 2025.
  - Explored fail rates by age (21% at 4 years, about 68% at 18–20), test year (the 2020 COVID spike), make and failure category.
  - Modelling: smoothed look-up baselines, logistic regression (brand and model ageing, year of birth, recency weighting) and a decision tree, tuned on 2024 and tested on 2025. Both models beat all baselines (group error about 4.1 against 5.0 percentage points) and are almost tied with each other.
  - Age-adjusted brand ranking (Toyota and Honda best; Citroën, Vauxhall, Renault and Peugeot worst) and k-means clustering of failure profiles, with a hybrid group that almost never fails on emissions.
  - Added a fail_probability() tool, a Deployment section and the conclusion.
- Project 4 (film recommender):
  - Downloaded MovieLens ml-latest-small (100,836 ratings, 610 users, 9,742 films) and listed 60 films I like in my_ratings.csv.
  - Verified the data and built a sparse user × film matrix (98.3% empty; 1.2 MB sparse against 47.5 MB dense).
  - Split each user's ratings by time (60/20/20) and evaluated top-10 lists with precision, recall, NDCG, hit rate, coverage and popularity.
  - Compared most-popular, content-based (TF-IDF of genres and tags), item-based collaborative filtering, matrix factorisation written in NumPy (SGD) and PureSVD. PureSVD and item-based CF tied as best (NDCG 0.107 against 0.076 for most popular). Rating-prediction MF had good RMSE (0.889) but ranked poorly.
  - Matched my films to MovieLens (51 of 60; 9 are newer than 2018) and produced personal recommendations with "because I liked" explanations.
- Next: prepare the presentation (week of 30 November) and final checks before the 27 November deadline.
