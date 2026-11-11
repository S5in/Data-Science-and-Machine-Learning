# Project 3: NCT Failure Prediction

**Question:** How likely is a car to fail its first NCT test, given its make, model and age? Which brands age best, and what do different models fail on?

**Data:** Road Safety Authority *Make and Model Data*, one file per test year from 2013 to 2025: 3 PDFs, 2 CSVs and 8 Excel files, all in [`data/`](data/). That's 93,233 rows covering 18.4 million initial NCT tests. The RSA's 2025 results by test centre are also included and used to cross-check the totals.

**Notebook:** [`nct_failures.ipynb`](nct_failures.ipynb) (HTML copy: `nct_failures.html`). It needs `pypdf` and `openpyxl` (`pip install -r ../requirements.txt`).

## Results

Models were trained on test years 2013–2023, tuned on 2024 and tested once on 2025. Each prediction is the fail rate of a make/model/year group:

| Method | Group error (percentage points) | Log loss | ROC AUC |
|---|---|---|---|
| Decision tree | **4.08** | 0.6320 | 0.696 |
| Logistic regression | 4.16 | **0.6319** | **0.698** |
| Baseline: same model and age | 4.98 | 0.6346 | 0.693 |
| Baseline: same make and age | 6.12 | 0.6389 | 0.688 |
| Baseline: same age | 7.17 | 0.6435 | 0.680 |
| Baseline: overall rate | 15.43 | 0.6931 | 0.500 |

- **Age is the main factor.** The fail rate rises from 21% at 4 years old to about 68% at 18–20.
- **Year of birth** was the most useful extra feature: newer cars fail less at the same age.
- **Brands, adjusted for age (2021–2025):** Toyota (0.90) and Honda (0.91) fail least for their age, and Citroën (1.19), Vauxhall (1.15), Renault (1.13) and Peugeot (1.12) fail most.
- **Clustering by failure profile (k-means and PCA):**
  - a hybrid-led group (Toyota Prius, Toyota C-HR, Honda Fit) with almost no emissions failures
  - a Hyundai/Kia-led group with more brake failures
  - a group that fails more on almost everything
  - an average group
- **A `fail_probability(make, model, year)` function** predicts the chance of failing the next NCT. For example, a 2015 Toyota Corolla has a 40% chance in 2026 and a 2015 Nissan Qashqai 63%.

## Data engineering

- Three file formats, with changing column names, are loaded into one tidy table. PDF tables are rebuilt from text with a regular expression, including long model names that wrap over several lines.
- Integrity checks: no missing values, pass + fail = total in every row, and no duplicates. The independent centre-level file confirms 98.9% coverage of 2025 initial tests.
- The RSA's privacy rules changed over time (groups of 1+ cars before 2021, 6+ from 2021, 11+ in 2025), so groups under 11 cars are removed from every year.

## Contents (CRISP-DM)

1. Business understanding
2. Setup
3. Data infrastructure: combining 13 years of files
4. Data understanding
5. Data quality summary
6. Provenance, licence and ethics
7. Data preparation
8. Modelling: baselines, logistic regression, decision tree, final 2025 test
9. Which brands age best?
10. Failure profiles: clustering
11. Using the model
12. Deployment
13. Conclusion
