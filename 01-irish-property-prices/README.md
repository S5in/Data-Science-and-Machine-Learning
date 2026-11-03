# Project 1: Irish Property Price Trends, 2010–2026

**Question:** How well can a regression model trained on past Irish house prices forecast how prices change over time, nationally and by county?

**Data:** [Residential Property Price Register](https://www.propertypriceregister.ie) (PSRA, CC BY 4.0). 807,936 sales, January 2010 to September 2026. The notebook reads it straight from `../archive.zip`.

**Notebook:** [`property_prices.ipynb`](property_prices.ipynb) (an HTML copy with all outputs is in `property_prices.html`)

## Key results

Models were trained on monthly median prices up to December 2023 and tested on January 2024 to September 2026.

| Model | Test RMSE | Test MAPE |
|---|---|---|
| Growth model + month dummies (multiple regression on log price, 2014+) | €8,143 | 1.8% |
| Lasso, degree-2 polynomial (tuned with time-series CV) | €8,537 | 1.9% |
| Growth model (linear regression on log price, 2014+) | €9,495 | 2.2% |
| Ridge, degree-2 polynomial (tuned with time-series CV) | €18,166 | 4.3% |
| Polynomial, degree 2 | €27,290 | 6.7% |
| Baseline (2023 average) | €57,112 | 13.7% |
| Linear regression | €62,349 | 16.1% |

- High-degree polynomials and Gaussian basis functions fit the training years best but forecast worst. They overfit and can't extrapolate.
- National prices grew by about 8% a year from 2014 to 2023, with a seasonal swing of about 5% (spring completions cheapest, autumn dearest).
- By county, growth ranged from 5.5% a year (Dublin) to 10.1% (Limerick).

## Contents (CRISP-DM)

1. Business understanding (incl. type of analytics)
2. Setup
3. Data understanding (incl. data formats, open data licence and provenance, data quality summary)
4. Data preparation (incl. ethics and privacy)
5. Exploratory analysis
6. Modelling: baseline, linear, polynomial, Ridge, Lasso, growth model, Gaussian basis functions, multiple regression with seasonality, coefficient interpretation
7. County-level models
8. Deployment
9. Conclusion
