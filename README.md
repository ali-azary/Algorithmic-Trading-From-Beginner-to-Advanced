# Algorithmic Trading From Beginner to Advanced – Strategy Pack

This repository contains a free mini-book and a curated pack of Backtrader strategies and utilities for learning and testing algorithmic trading ideas.

Everything is designed to be readable, hackable, and educational: you can study the code, modify the parameters, and run backtests on your own data.

## Contents

- `book/Algorithmic Trading From Beginner to Advanced.pdf`  
  A practical PDF guide that walks through the ideas behind these strategies and how to use them.

- `codes/`  
  A collection of complete Backtrader strategies and helper scripts:
  - Single-asset backtest runner
  - Rolling (walk-forward style) backtester
  - Stress testing system for “what if” scenarios

## Strategies included

All strategies are implemented as `bt.Strategy` subclasses in `codes/` and use `yfinance` data plus Backtrader analyzers.

- **IchimokuCloudStrategy.py**  
  Trades confirmed breakouts from the Ichimoku Cloud (Kumo). Combines:
  - Price breakout from the cloud  
  - Tenkan/Kijun cross for momentum  
  - Chikou Span confirmation  
  - ATR-style trailing stop for exits

- **KeltnerBreakoutStrategy.py**  
  Classic Keltner Channel breakout:
  - Custom `KeltnerChannel` indicator (EMA centerline + ATR bands)  
  - Enters when price breaks out of the channel  
  - Uses ATR-based risk management and position handling

- **KeltnerChannelRSIBreakoutStrategy.py**  
  Keltner breakout + RSI confirmation:
  - EMA + ATR Keltner bands  
  - Long when price > upper band and RSI confirms strength  
  - Short when price < lower band and RSI confirms weakness  
  - ATR-based trailing stop

- **MLEnhancedADXStrategy.py**  
  ADX trend strength strategy enhanced with machine learning:
  - Uses ADX, RSI, ATR and other features  
  - Trains a `RandomForestClassifier` (scikit-learn) on rolling windows  
  - Filters trades using the ML model’s predicted probability  
  - ATR-based trailing stops and risk management

- **MomentumIgnitionStrategy.py**  
  “Consolidation → ignition” momentum breakout:
  - Detects low-volatility consolidation using standard deviation  
  - Enters on a statistical breakout in ROC (Rate of Change)  
  - Requires alignment with long-term trend  
  - Uses ATR-based stop loss

- **OBVMarketRegimeStrategyBreakout.py**  
  Volume-based regime and breakout strategy:
  - Custom On-Balance Volume (`CustomOBV`) indicator  
  - Classifies market regimes (e.g. accumulation / distribution)  
  - Enters on OBV + price breakouts in favorable regimes  
  - Uses ATR-style risk control

- **OBVmomentumStrategy.py**  
  OBV-driven momentum strategy:
  - Custom OBV with moving average smoothing  
  - RSI and volume filters for signal quality  
  - Trailing percent stop to lock in profits

- **OUMeanReversionStrategy.py**  
  Ornstein–Uhlenbeck mean reversion on a single asset:
  - Estimates OU process parameters on a rolling window  
  - Converts deviations into a z-score  
  - Enters mean-reversion trades when |z| exceeds entry threshold  
  - Exits when z-score mean-reverts  
  - Includes optional SMA filter and detailed logging

- **QuantileChannelStrategy.py**  
  Quantile regression channel strategy:
  - Uses a custom `QuantileRegression` class to fit channels around price  
  - Channels behave like statistically grounded support/resistance  
  - Can be used for breakout or mean-reversion style entries

- **RegimeFilteredTrendStrategy.py**  
  Trend-following with explicit regime filters:
  - Fast/slow moving averages for trend direction  
  - ADX for trend strength  
  - Bollinger Band width and realized volatility as regime filters  
  - Different ATR-based trailing logic for trending vs ranging markets  
  - Dynamic position sizing based on regime

- **RelativeMomentumAccel.py**  
  Adaptive momentum acceleration breakout:
  - Baseline trend via KAMA (Kaufman’s Adaptive Moving Average)  
  - Fast EMA vs KAMA to compute a “thrust” oscillator  
  - Bollinger Bands on the oscillator for statistical breakouts  
  - ATR-based risk management

## Utility scripts

Located in `codes/`:

- **backtest.py**  
  Single backtest on one asset and one strategy using `yfinance` data.

  Key points:
  - Edit the script to set:
    - `ticker` (e.g. `"BTC-USD"`)  
    - `start` / `end` dates  
    - Strategy class name in  
      ```python
      strategy = load_strategy("QuantileChannelStrategy")
      ```
      Change `"QuantileChannelStrategy"` to any other strategy class name in `codes/`.
  - Uses Backtrader analyzers:
    - Sharpe ratio  
    - Drawdown  
    - CAGR / total return  
    - Trades statistics  
  - Plots equity curve and price using Backtrader’s built-in plotting.

- **rolling_backtest.py**  
  Rolling / walk-forward styled backtesting on a single asset:
  - Uses sliding windows (e.g. N months in-sample, then next N months out-of-sample)  
  - Runs the chosen strategy on each window  
  - Aggregates performance metrics  
  - Plots period-by-period returns with green/red bars and summary stats  

  Like `backtest.py`, you set the strategy via:

  ```python
  strategy = load_strategy("KeltnerBreakoutStrategy")
