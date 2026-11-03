# Project 1: Irish Property Price Trends, 2010–2026

**Question:** How well can a regression model trained on past Irish house prices forecast how prices change over time, nationally and by county?

**Data:** [Residential Property Price Register](https://www.propertypriceregister.ie) (PSRA). 807,936 sales, January 2010 to September 2026. The notebook reads it straight from `../archive.zip`.

**Notebook:** [`property_prices.ipynb`](property_prices.ipynb) (an HTML copy with all outputs is in `property_prices.html`)

## Key results

Models were trained on monthly median prices from 2010 to 2023 and tested on January 2024 to September 2026.

| Model | Test RMSE | Test MAPE |
|---|---|---|
| Lasso, degree-2 polynomial (tuned with time-series CV) | €8,537 | 1.9% |
| Growth model (linear regression on log price, 2014+) | €9,495 | 2.2% |
| Ridge, degree-2 polynomial (tuned with time-series CV) | €18,166 | 4.3% |
| Polynomial, degree 2 | €27,290 | 6.7% |
| Baseline (2023 average) | €57,112 | 13.7% |
| Linear regression | €62,349 | 16.1% |

- High-degree polynomials fit the training years best but forecast worst (overfitting).
- National prices grew by about 8% a year from 2014 to 2023. By county, growth ranged from 5.5% (Dublin) to 10.1% (Limerick).

## Progress

- [x] Business understanding
- [x] Data understanding
- [x] Data preparation (market-price filter, duplicates, VAT adjustment, outliers, monthly series)
- [x] Exploratory analysis (national trend, counties, new vs second-hand)
- [x] Modelling: baseline, linear, polynomial, Ridge, Lasso, growth model
- [x] Evaluation on 2024–2026 and county-level models
- [x] Conclusion and limitations
- [ ] Final portfolio: extra features (new-home share, interest rates), models from later in the module
