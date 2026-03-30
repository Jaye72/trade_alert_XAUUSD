# 🥇 XAUUSD Gold Trading Signal Bot

> An automated trading signal monitor for Gold (XAUUSD) on the **15-minute timeframe**, sending real-time alerts directly to your Telegram.

---

## 📊 MT5 Chart Preview

![MT5 XAUUSD 15M Chart](screenshots/mt5_chart.png)
*MetaTrader 5 — XAUUSD 15M chart with EMA 50, Stochastic (5,3,3), and RSI (14) active*

---

## Overview

This bot connects to MetaTrader 5, reads live 15-minute candle data for XAUUSD, runs multiple technical strategies, and pushes formatted signal alerts to your Telegram chat — automatically, every time a new candle forms with an active signal.

---

## ⚙️ How It Works

1. **Connects to MetaTrader 5** and fetches the last 100 candles of XAUUSD on the 15M timeframe every 5 seconds.
2. **Calculates three technical indicators** fresh on every check: EMA 50, Stochastic Oscillator (5,3,3), and RSI (14).
3. **Runs all four signal strategies** against the latest candle values.
4. **Checks for a new candle** — if the current candle's open time matches the last recorded one, the alert is suppressed to avoid duplicate notifications.
5. **Formats and sends a Telegram message** when a new candle has one or more active signals.

---

## 📐 Strategies

### 1. EMA 50 Touch
Triggers when the current closing price comes within **0.1%** of the 50-period Exponential Moving Average. The EMA 50 acts as a dynamic support/resistance level — price touching it often signals a potential bounce or breakout.

### 2. Stochastic Oscillator (5, 3, 3)
Triggers when both the K line and D line are simultaneously:
- **Above 80** → `STOCHASTIC_OVERBOUGHT` — market may be due for a pullback
- **Below 20** → `STOCHASTIC_OVERSOLD` — market may be due for a recovery

### 3. RSI (14)
Triggers when the Relative Strength Index crosses momentum extremes:
- **Above 70** → `RSI_OVERBOUGHT`
- **Below 30** → `RSI_OVERSOLD`

### 4. RSI + Stochastic Combo (Strong Signal)
The most powerful signal — fires only when **both** RSI and Stochastic hit their extremes at the same time:
- RSI ≤ 30 **AND** Stochastic K & D ≤ 20 → `RSI_STOCHASTIC_OVERSOLD_COMBO` 🟢 🔥
- RSI ≥ 70 **AND** Stochastic K & D ≥ 80 → `RSI_STOCHASTIC_OVERBOUGHT_COMBO` 🔴 🔥

Two independent indicators agreeing simultaneously makes this a much higher-confidence setup than either signal alone.

---

## 📲 Telegram Alert Format

```
🚨 TRADING SIGNAL ALERT 🚨
Time: 2025-01-15 10:30:00
Symbol: XAUUSD
Timeframe: 15M

Price:       2345.123
EMA 50:      2341.876
Stochastic:  K=82.4, D=81.1
RSI:         71.3

ACTIVE SIGNALS:
🔴 RSI_OVERBOUGHT
🔴 🔥 RSI_STOCHASTIC_OVERBOUGHT_COMBO (STRONG REVERSAL)
```

Signal color coding: 🔴 overbought · 🟢 oversold · 🟡 EMA touch · 🔥 combo (strong reversal)

### Telegram Screenshot

<!-- Add your Telegram alert screenshot here -->
> 📸 *Screenshot coming soon — replace this block with:*
> `![Telegram Alert](screenshots/telegram_alert.png)`

---

## 🛡️ Duplicate Alert Prevention

The bot tracks the open time of the last candle for which an alert was sent. Even though it checks the market every 5 seconds, a Telegram message is only dispatched **once per 15-minute candle** — keeping your notifications clean and actionable.

---

## 🛠️ Requirements

- Python 3.8+
- MetaTrader 5 terminal installed and running
- A funded or demo XAUUSD account (symbol: `XAUUSDm`)
- A Telegram bot token and chat ID

```bash
pip install MetaTrader5 pandas requests
```

---

## 🚀 Usage

```bash
python gold_test_ALL.py
```

For a one-time snapshot analysis instead of continuous monitoring, uncomment `get_detailed_analysis()` at the bottom of the file and comment out `multi_strategy_monitor()`.

---

## ⚠️ Disclaimer

This bot is for **informational and educational purposes only**. Signals generated do not constitute financial advice. Always apply your own risk management before placing any trade.
