# Applying AI and Machine Learning in Stock Investment

This repository contains a university project that explores a data-driven workflow for ranking S&P 500 stocks and predicting next-week returns. It combines weekly Refinitiv stock and fundamental data with S&P 500 returns and the CBOE Volatility Index (VIX), then applies tree-based machine-learning models to identify stocks with a higher probability of outperforming their peers.

This project was completed as part of the **Advanced Seminar** coursework at the **TUM School of Management, TUM Campus Heilbronn**, under the supervision of **Prof. Dr. Sebastian Müller**, Professor of Finance, and **Christian Breitung**, PhD candidate.

## Project workflow

The analysis is organized into two Jupyter notebooks:

1. **Historical model development (`Campus Challenge.ipynb`)**
   - Loads approximately ten years of weekly stock data from the `full` worksheet in `Request finish.xlsm`.
   - Reshapes the source data from wide to long format.
   - Cleans errors and missing values and merges the stock data with weekly S&P 500 returns and VIX levels.
   - Engineers rolling return features over 4, 8, 12, and 24 weeks, plus a 24-week Sharpe ratio, skewness, and kurtosis.
   - Labels stocks according to whether their next-week return is above the cross-sectional weekly median.
   - Trains a Random Forest classifier and ranks companies by predicted outperformance probability.
   - Selects the top 30 companies and predicts next-week returns with Random Forest regression and a stacking ensemble.

2. **Updated Week 1 data preparation (`Code_part 2.ipynb`)**
   - Loads the selected-stock data from the `Week_1` worksheet in `Request table_2024.12.30.xlsm`.
   - Repeats the cleaning, reshaping, market-data merge, feature engineering, and standardization steps for the updated period.
   - Produces the standardized feature set and next-week-return target used for the next stage of analysis.

## Repository contents

| File | Description |
| --- | --- |
| `Campus Challenge.ipynb` | Main historical analysis, stock ranking, and return-prediction notebook |
| `Code_part 2.ipynb` | Updated Week 1 data-preparation notebook |
| `Request finish.xlsm` | Historical Refinitiv stock and fundamental dataset; the notebook reads the `full` worksheet |
| `Request table_2024.12.30.xlsm` | Updated selected-stock dataset; the notebook reads the `Week_1` worksheet |
| `weekly_sp500_vix.csv` | Weekly S&P 500 returns and VIX levels for the historical analysis |
| `weekly_sp500_vix_wk1.csv` | Updated weekly S&P 500 and VIX data for the Week 1 analysis |

## Methods

### Features

The notebooks use market, valuation, profitability, liquidity, leverage, and macro-market variables. Examples include prices, trading volume, VWAP, P/E, price-to-book value, dividend yield, margins, ROE, ROIC, ROA, EBITDA, liquidity ratios, debt ratios, S&P 500 returns, and VIX levels.

Seven time-series features are added to the source variables:

- rolling average returns over 4, 8, 12, and 24 weeks;
- 24-week Sharpe ratio;
- 24-week skewness; and
- 24-week kurtosis.

### Models and metrics

- **Random Forest classification** predicts the probability that a stock's next-week return will exceed the weekly median. Performance is evaluated with classification accuracy.
- **Random Forest regression** predicts next-week returns for the selected stocks. Performance is evaluated with mean squared error (MSE) and R-squared.
- **Stacking regression** combines Random Forest, Gradient Boosting, and Linear Regression base learners with a Gradient Boosting meta-model.

## Getting started

### 1. Clone the repository

```bash
git clone <repository-url>
cd TUM-Project_Applying-AI-and-Large-Language-Models-in-Stock-Investment
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

### 3. Install the dependencies

```bash
python -m pip install jupyter pandas numpy scikit-learn openpyxl yfinance
```

`yfinance` is only needed if you uncomment the market-data download cells. The repository already includes the two generated CSV files used by the notebooks.

### 4. Run the notebooks

Start Jupyter from the repository root so that the relative data paths resolve correctly:

```bash
jupyter notebook
```

Run `Campus Challenge.ipynb` first, followed by `Code_part 2.ipynb` for the updated-data workflow.

## Runtime notes

- The classifier grid-search cell in `Campus Challenge.ipynb` is enclosed in a multiline string and is therefore disabled by default.
- The Random Forest regression randomized search is active and tests 100 parameter combinations with three-fold cross-validation. It can take a long time on a typical laptop. Skip that cell if you only want to reproduce the analysis with the saved best parameters shown in the following cell.
- The notebooks contain saved cell outputs, so results can be reviewed without rerunning the expensive searches.
- The Excel workbooks are macro-enabled. They are opened with `openpyxl` using `keep_vba=True`; the notebooks read worksheet values but do not execute macros.

## Data sources

- Stock prices, fundamentals, and company data: **Refinitiv**
- S&P 500 index (`^GSPC`) and VIX (`^VIX`) market data: **Yahoo Finance**, downloaded through `yfinance`

The included data may be subject to the original providers' terms and licensing restrictions. Verify that you have permission before redistributing or using it outside this project.

## Limitations

- The train/test splits are sample-based rather than strict walk-forward backtests, so the reported metrics should not be interpreted as live trading performance.
- Transaction costs, slippage, liquidity constraints, taxes, and survivorship bias are not fully modeled in the included notebooks.
- The repository does not currently implement portfolio-weight optimization or an end-to-end trading execution system.
- Results are for research and educational purposes only and are not financial advice.

## Technology stack

Python, Jupyter Notebook, pandas, NumPy, scikit-learn, openpyxl, and yfinance.
