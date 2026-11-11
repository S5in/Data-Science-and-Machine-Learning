# Data Science & Machine Learning Portfolio

**Author:** Maksym Redchenko · **Student number:** C00302240
BSc (Hons) Software Development, SETU Carlow
[LinkedIn](https://www.linkedin.com/in/maksym-redchenko) · [GitHub](https://github.com/S5in)

A portfolio of data science and machine learning projects built on real, public data.

## Projects

| # | Project | Question | Status |
|---|---|---|---|
| 1 | [Irish Property Price Trends](01-irish-property-prices/) | How well can regression on past prices forecast Irish house price trends? | Complete |
| 2 | [UFC Fight Outcome Prediction](02-ufc-fight-prediction/) | Can engineered pre-fight features predict fight winners better than a baseline? | Complete |
| 3 | [NCT Failure Prediction](03-nct-failure-prediction/) | How likely is a car to fail its NCT, given its make, model and age? | Complete |

## Repository structure

```
archive.zip                    Property Price Register data (CSV, JSON, Parquet)
ufc_archive.zip                UFC fight data scraped from ufcstats.com
01-irish-property-prices/      Project 1 notebook and notes
02-ufc-fight-prediction/       Project 2 notebook (builds a SQLite database, ufc.db)
03-nct-failure-prediction/     Project 3 notebook and the 13 yearly RSA files (data/)
LOG.md                         Weekly work log
requirements.txt               Python packages needed
```

## How to run

```
pip install -r requirements.txt
jupyter notebook
```

Then open a project's notebook and use **Kernel → Restart & Run All**. Projects 1 and 2 read their data straight from the zip files, and Project 3 reads the RSA files in its `data/` folder, so nothing needs to be extracted.

## Technologies

Python, pandas, NumPy, scikit-learn, matplotlib, Jupyter, Git/GitHub, SQLite (Project 2) and pypdf for PDF table extraction (Project 3). See the portfolio specification for the reasons behind each choice.
