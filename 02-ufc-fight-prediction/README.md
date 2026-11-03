# Project 2: UFC Fight Outcome Prediction

**Question:** Can engineered features built only from information known before a fight predict its winner better than a simple baseline?

**Data:** UFC fight statistics scraped from [ufcstats.com](http://www.ufcstats.com) (11 August 2026). 11,441 fights from November 1993 to August 2026, 4,581 fighters and 25,131 rounds. The notebook reads it from `../ufc_archive.zip`.

**Notebook:** [`ufc_fights.ipynb`](ufc_fights.ipynb) (HTML copy: `ufc_fights.html`). Running it creates `ufc.db`, a SQLite database (not committed, because it can be rebuilt).

## Findings so far

- **Relational database:** six normalised tables with primary keys, foreign keys and CHECK constraints. All rows loaded with zero foreign-key violations.
- **Red-corner problem:** before April 2010, the fighter listed as "red" wins almost every fight, because older records list the winner first. Fighter order will be randomised so the model can't exploit this.
- **Leakage trap:** fighter career statistics are a 2026 snapshot that includes future fights. They will be rebuilt from earlier fights only.
- **Missing round statistics:** 330 fights (mostly 2006–2011) have no round data, and `master.csv` fills them with zeros. The database leaves them missing instead.
- **The sport has changed:** finish rate fell from almost 100% in the mid-1990s to about 50% today.

## Progress

- [x] Business understanding
- [x] Data infrastructure: SQLite database (ETL, schema, constraints, verification)
- [x] Data understanding (SQL queries)
- [x] Data quality summary
- [x] Provenance, licence and ethics (Kaggle source and licence still to add)
- [ ] Feature engineering: pre-fight records, Elo rating, rolling statistics
- [ ] Modelling: baselines, logistic regression, later classification models
- [ ] Evaluation (chronological test set, 2024–2026) and conclusion
