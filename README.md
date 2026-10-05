# Financial Markets: Event Study and Downside Beta

Two empirical exercises in asset pricing, written as Jupyter notebooks in Python (pandas, statsmodels, matplotlib).

## Projects

### 1. CPI announcement event study
[`cpi_announcement_event_study.ipynb`](cpi_announcement_event_study.ipynb)

Tests whether U.S. stock market returns are higher on days when CPI inflation numbers are released, in the same spirit as the literature on FOMC announcement returns.

- **Market proxy:** SPY daily total return (dividends included), from CRSP.
- **Sample:** 2021–2025 (1,255 trading days, 59 CPI release days).
- **Method:** regress daily returns on a CPI-day dummy, `Return_t = α + β·CpiDay_t + ε_t`, with heteroskedasticity-robust (HC1) standard errors and a one-sided test of β > 0.
- **Result:** the average return is 0.118% on CPI days and 0.056% on other days. The difference is positive but not statistically significant (one-sided p = 0.375).

[`cpi_event_study_2010_2025.ipynb`](cpi_event_study_2010_2025.ipynb) repeats the test on a longer 2010–2025 sample. There the CPI-day return is slightly *lower* than on other days, and the difference is again insignificant.

### 2. Upside and downside beta
[`downside_beta_industry_portfolios.ipynb`](downside_beta_industry_portfolios.ipynb)

Compares three versions of CAPM beta to see which best explains average returns across the 17 Fama-French industry portfolios.

- **Sample:** daily data, 2010–2024.
- **β:** slope from regressing industry excess returns on the market excess return, using all days.
- **β⁺ / β⁻:** the same regression using only days when the market excess return is at least μ, or below μ, with μ = 0.
- **Result:** all three slopes against average returns are positive. Downside beta fits best (cross-sectional R² = 0.136, compared with 0.092 for β and 0.078 for β⁺), which is consistent with downside-risk pricing (Ang, Chen & Xing, 2006). With only 17 portfolios, none of the slopes is statistically significant.

## Data

All input files go in [`data/`](data/).

| File | Source | In repo |
|---|---|---|
| `17_Industry_Portfolios_Daily.csv` | [Ken French Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html) | Yes |
| `F-F_Research_Data_Factors_daily.csv` | Ken French Data Library (Fama/French 3 Factors, daily) | Yes |
| `release_dates_10.csv`, `release_dates_15.csv` | Inflation release dates, from the BLS release schedule | Yes |
| `SPY_data.csv`, `SPY_data2.csv` | CRSP daily stock file via [WRDS](https://wrds-www.wharton.upenn.edu/) | **No** (licensed) |

The CRSP files can't be shared publicly under the WRDS terms. To rebuild them, download daily returns for SPY (PERMNO 84398) from the CRSP daily stock file on WRDS with these columns: `PERMNO, HdrCUSIP, Ticker, PERMCO, DlyCalDt, DlyRet`.
- `SPY_data.csv`: 2020–2025
- `SPY_data2.csv`: 2010–2025

The Ken French files had their description lines removed from the top. The notebooks read only the value-weighted table and skip the footer.

## Running

```bash
pip install pandas numpy statsmodels matplotlib jupyter
jupyter notebook
```

Run each notebook from the repository root so the `data/` paths resolve.
