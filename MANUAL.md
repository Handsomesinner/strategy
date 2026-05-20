# MTF ICT/SMC Strategy — User Manual

---

## What This Strategy Does

This is a **top-down multi-timeframe trading strategy** built on ICT (Inner Circle Trader) and Smart Money Concepts (SMC). It analyses 5 timeframes simultaneously — Weekly, Daily, 4-Hour, 1-Hour, and 5-Minute — and only generates a trade signal when **all conditions align at the same time**.

It runs on **TradingView** and works on **any Forex pair** (EURUSD, GBPUSD, USDJPY, etc.) as well as indices and crypto.

---

## Requirements

| Item | Requirement |
|---|---|
| Platform | TradingView (free or paid) |
| Chart Timeframe | Any — M5, M15, M30, 1H, 4H recommended |
| Market | Forex (any pair), Indices, Crypto |
| Pine Script Version | v6 |

---

## How to Install

1. Open **TradingView** and load any Forex chart (e.g. EURUSD)
2. Switch the chart to **1H** timeframe
3. Click **Pine Editor** at the bottom of the screen
4. Delete any existing code in the editor
5. Copy the full contents of `mtf_ict_smc_strategy.pine` and paste it in
6. Click **Add to chart**
7. The strategy will load with OB boxes, session backgrounds, and the info table

---

## Understanding the Chart

Once loaded, you will see the following on your chart:

### Order Block (OB) Boxes
- **Red box** → Bearish Order Block (institutional selling zone)
- **Green box** → Bullish Order Block (institutional buying zone)
- **Grey box** → Mitigated OB (price has already passed through it — no longer valid)

### Session Background Colours
- **Light blue columns** → London Session (07:30 – 09:45 UTC)
- **Light orange columns** → New York Session (13:30 – 15:45 UTC)
- Trades are only taken during these windows

### Trade Level Lines
- **Red solid line** → Stop Loss level
- **Green dashed line** → Take Profit level
- These appear automatically when a trade is active

### Signal Arrows
- **Green arrow (↑) below a candle** → Long (Buy) signal
- **Red arrow (↓) above a candle** → Short (Sell) signal

---

## The Info Table (Top-Left Corner)

This is the most important thing to watch. It shows the live status of every condition in real time.

```
┌──────────────────┬────────────────────┐
│ Weekly Bias      │ BULLISH ▲          │
│ Daily Structure  │ Bullish HH/HL      │
│ 4H Confirm       │ Bullish ▲          │
│ Daily OB         │ ✓ Tapped (Bull)    │
│ M5 Sweep         │ ✓ Swept            │
│ Session          │ ✓ Active           │
│ Position         │ Flat               │
└──────────────────┴────────────────────┘
```

### What Each Row Means

| Row | What It Checks | Ready When It Shows |
|---|---|---|
| **Weekly Bias** | Overall market direction from the weekly chart | `BULLISH ▲` or `BEARISH ▼` |
| **Daily Structure** | Is the daily chart making higher highs/lows (bull) or lower highs/lows (bear) | `Bullish HH/HL` or `Bearish LH/LL` |
| **4H Confirm** | Is the 4-hour chart positioned in the same direction as the weekly bias | `Bullish ▲` or `Bearish ▼` |
| **Daily OB** | Has price tapped into a valid Daily Order Block in the bias direction | `✓ Tapped (Bull)` or `✓ Tapped (Bear)` |
| **M5 Sweep** | Has the 5-minute chart taken liquidity (swept a swing high/low) confirming a reversal | `✓ Swept` |
| **Session** | Is the current time inside London or New York session | `✓ Active` |
| **Position** | Current trade status | `Long ▲`, `Short ▼`, or `Flat` |

### Colour Guide
- **Green text** = condition is met ✓
- **Grey text** = condition not yet met, waiting
- **Red text** = bearish condition active

**A signal fires the moment all 6 rows turn green at the same time.**

---

## The Logic — Step by Step

The strategy works like a checklist from the highest timeframe down:

```
Step 1 — WEEKLY
Is the market bullish or bearish this week?
(Checks: external breaks, swing structure, pin bars, engulfing candles)
         ↓
Step 2 — DAILY
Is the daily structure confirming the weekly direction?
Is there an unmitigated Order Block that price is approaching?
         ↓
Step 3 — 4H
Is the 4-hour chart also positioned in the same direction?
(Price above the 4H range midpoint = bullish, below = bearish)
         ↓
Step 4 — 1H  (the chart timeframe)
Has a strong intent candle formed at the Order Block?
(A candle body larger than 0.8 × ATR signals institutional intent)
         ↓
Step 5 — M5
Has the 5-minute chart swept liquidity (false break of a swing point)?
This confirms smart money has collected liquidity before the move
         ↓
Step 6 — SESSION
Is this happening during London (07:30–09:45) or NY (13:30–15:45) UTC?
These are the only windows with enough volume for clean moves
         ↓
SIGNAL FIRES → Arrow appears + SL/TP lines drawn + Alert triggers
```

---

## Entry, Stop Loss & Take Profit

| | Long (Buy) | Short (Sell) |
|---|---|---|
| **Entry** | Market order at signal bar close | Market order at signal bar close |
| **Stop Loss** | Below the bottom of the bullish OB | Above the top of the bearish OB |
| **Take Profit** | Nearest daily inducement level above entry (or 2:1 RR) | Nearest daily inducement level below entry (or 2:1 RR) |

The SL and TP lines are drawn automatically on the chart when a trade opens.

---

## Setting Up Alerts (So You Don't Miss a Signal)

You do not need to watch the chart all day. Set up alerts and TradingView will notify you.

1. Right-click anywhere on the chart
2. Select **Add Alert**
3. Under **Condition**, select `MTF ICT/SMC Strategy [1H]`
4. Choose one of the following:

| Alert Name | When It Fires |
|---|---|
| `MTF Long Signal` | All 6 conditions met — buy signal ready |
| `MTF Short Signal` | All 6 conditions met — sell signal ready |
| `Daily OB Tapped (Pre-Confirmation)` | Price has entered an OB — watch for full confirmation |
| `Stop Loss Hit` | Active trade has been stopped out |
| `Take Profit Hit` | Active trade has reached the target |

5. Set your notification method: **Popup**, **Email**, or **App notification**
6. Click **Create**

---

## Settings You Can Adjust

Click the **⚙ Settings** icon on the strategy name to adjust these inputs:

### Weekly Settings
| Input | Default | Description |
|---|---|---|
| Weekly Pivot Lookback | 3 | How many candles left/right to confirm a weekly swing |

### Daily Settings
| Input | Default | Description |
|---|---|---|
| Daily Pivot Lookback | 5 | How many candles to confirm a daily swing |
| Max OB Boxes on Chart | 10 | Maximum number of OB boxes drawn |

### 4H Settings
| Input | Default | Description |
|---|---|---|
| 4H Range Lookback | 20 | Number of 4H bars used to calculate the range |

### 1H Settings
| Input | Default | Description |
|---|---|---|
| ATR Period | 14 | Period for Average True Range calculation |
| Intent Candle ATR Mult | 0.8 | How large the 1H candle body must be (× ATR) to count as intent |
| SL Buffer (ticks) | 5 | Extra ticks added beyond the OB for the stop loss |

### Session Filter
| Input | Default | Description |
|---|---|---|
| London Open | 07:30 UTC | Start of London session window |
| London Close | 09:45 UTC | End of London session window |
| NY Open | 13:30 UTC | Start of New York session window |
| NY Close | 15:45 UTC | End of New York session window |
| Use Session Filter | On | Turn off to allow signals outside session hours |

### Visuals
| Input | Default | Description |
|---|---|---|
| Show Weekly Levels | On | Dotted lines for weekly swing high/low |
| Show Daily OB Boxes | On | Green/red OB zones |
| Show 4H Range Lines | On | Dashed lines for the 4H range high, low, and midpoint |
| Show Session Background | On | Coloured column backgrounds for London/NY |
| Show Info Table | On | The status table in the top-left corner |

---

## Common Questions

**Why is the table showing "No Structure" for Daily?**
The daily chart has not yet formed a clear higher-high/higher-low or lower-high/lower-low pattern. Wait for the structure to develop before looking for entries.

**Why is the Weekly Bias showing "NEUTRAL"?**
Not enough weekly candle data has been processed yet, or the weekly chart has no clear directional break. Zoom out on your chart to make sure there is enough history loaded.

**Why is the M5 Sweep showing "Waiting" even though I can see a sweep?**
The M5 sweep uses confirmed closed bars. If the current 1H bar is still forming, the M5 data updates at the close of each 1H candle. Wait for the 1H bar to close.

**The strategy isn't taking trades. Is it broken?**
No — this is by design. The strategy only enters when all 6 conditions align simultaneously. This is intentional to reduce low-quality setups. A quiet period with no signals is normal and healthy.

**Can I use this on other timeframes?**
Yes. The strategy works on M5, M15, M30, 1H, and 4H charts. The Weekly, Daily, 4H, and M5 reference data is always fetched automatically regardless of the chart timeframe. The info table will show your current timeframe in the header.

---

## Disclaimer

This strategy is a technical analysis tool and does not guarantee profitable results. Always use proper risk management, trade with money you can afford to lose, and consider practising on a demo account before trading live.

---

*Strategy built with Pine Script v6 on TradingView.*
