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
- Next: add the Kaggle source and licence, and try the classification models covered later in the module on the same features.

