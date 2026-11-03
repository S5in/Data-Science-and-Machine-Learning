# Project 2: UFC Fight Outcome Prediction

**Question:** Can engineered features built only from information known before a fight predict its winner better than a simple baseline?

**Data:** fight statistics scraped from [ufcstats.com](http://www.ufcstats.com) on 11 August 2026. 11,441 fights from November 1993 to August 2026, 4,581 fighters and 25,131 rounds, including fights from older promotions (PRIDE, Strikeforce, WEC and others) that are listed on the site. The notebook reads it from `../ufc_archive.zip`.

**Notebook:** [`ufc_fights.ipynb`](ufc_fights.ipynb) (HTML copy: `ufc_fights.html`). Running it creates `ufc.db`, a SQLite database (not committed, because it can be rebuilt).

## Results

Trained on fights from April 2010 to December 2023, and tested on 1,528 fights from January 2024 to August 2026:

| Method | Test accuracy |
|---|---|
| **Logistic regression (pre-fight features)** | **64.7%** (log loss 0.635, ROC AUC 0.693) |
| Pick the better win rate | 60.1% |
| Pick the younger fighter | 60.1% |
| Pick the higher Elo rating | 58.4% |
| Coin flip | 49.3% |
| Pick the more experienced fighter | 45.6% |

The model's probabilities are well calibrated. Elo rating and age have the largest effects.

## Data problems found and fixed

- **Red corner:** before April 2010, the fighter listed as "red" wins almost every fight, because older records list the winner first. **Fixed:** only fights from April 2010 are modelled, fighters are assigned to A/B at random (fighter A wins 50.3%), and corner colour is never used.
- **Leakage:** fighter career statistics are a 2026 snapshot that includes future fights. **Fixed:** every feature is rebuilt from fights on earlier dates only. A demonstration shows the snapshot statistics would give a misleading 70.7% accuracy.
- **Early rules:** the near-100% finish rate in 1993–1994 is real, not an error. Those events had no time limits, so decisions were impossible. **Fixed:** finish-rate analysis and models use fights from 2001 onwards, when round-based rules with judges became standard (finish rate about 70% in 2001–2008, about 52% since 2010).
- **Missing round statistics:** 330 fights have no round data, and `master.csv` fills them with zeros. **Fixed:** the database leaves them missing.
- **Also fixed:** 154 weight-class labels mapped to 12 standard divisions, one impossible date of birth, and extreme per-minute rates from very short careers (a 5-minute minimum).

## Contents (CRISP-DM)

1. Business understanding
2. Setup
3. Data infrastructure: SQLite database (ETL, schema, keys and constraints, verification)
4. Data understanding with SQL (incl. rules eras, red-corner problem, leakage, promotions)
5. Data quality summary
6. Provenance, licence and ethics (*Kaggle source and licence still to add*)
7. Data preparation (SQL view, standard divisions, one row per fighter per fight)
8. Feature engineering (date-strict history, streaks, Elo rating with K chosen on training data)
9. Modelling (red-corner fix, baselines, logistic regression, calibration, coefficients, leakage demonstration)
10. Conclusion
