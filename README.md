# 🔴 RSI Exhaustion Bands Alert — `RSI_Exhaustion_Bands_Alert_001.mq4`

> **MQL4 Script for MetaTrader 4**  
> Monitors RSI across two tiers of threshold zones — standard overbought/oversold and extreme exhaustion bands — firing layered real-time alerts as momentum enters each zone.

---

## Overview

Standard RSI alerts fire at 70 and 30. But not all RSI readings above 70 are equal — there's a significant difference between RSI at 72 (moderately overbought) and RSI at 88 (deeply exhausted momentum). This script introduces a **two-tier alert system**:

- **Standard zones** (configurable, default 70/30) — RSI entering typical overbought/oversold territory
- **Exhaustion zones** (configurable, default 85/15) — RSI reaching extreme momentum exhaustion, where reversal probability is highest

By distinguishing between these two levels, traders receive a graded signal — a moderate warning at 70, and an elevated warning at 85 — allowing for proportionally scaled responses.

---

## How It Works

Every 60 seconds the script fetches the current RSI value and evaluates it against four thresholds in priority order:

```
RSI >= ExhaustionUpper (85) → "RSI in Extreme Overbought Zone"
RSI <= ExhaustionLower (15) → "RSI in Extreme Oversold Zone"
RSI >= OverboughtLevel (70) → "RSI in Standard Overbought Zone"
RSI <= OversoldLevel   (30) → "RSI in Standard Oversold Zone"
```

Exhaustion checks take priority — if RSI is at 88, only the exhaustion alert fires (not the standard alert).

---

## Input Parameters

| Parameter | Default | Type | Description |
|---|---|---|---|
| `TradeSymbol` | `"EURUSD"` | string | Symbol to monitor |
| `Timeframe` | `PERIOD_H1` | ENUM_TIMEFRAMES | Timeframe for RSI calculation |
| `RSIPeriod` | `14` | int | RSI calculation period |
| `OverboughtLevel` | `70` | double | Standard overbought threshold |
| `OversoldLevel` | `30` | double | Standard oversold threshold |
| `ExhaustionUpper` | `85` | double | Extreme overbought (exhaustion) threshold |
| `ExhaustionLower` | `15` | double | Extreme oversold (exhaustion) threshold |
| `EnableAlerts` | `true` | bool | Trigger MT4 sound alerts |
| `EnableEmail` | `false` | bool | Send email notifications |
| `EnablePush` | `false` | bool | Send push notifications to mobile |

---

## Alert Signals

**Standard zone alerts:**
```
RSI in Standard Overbought Zone detected on EURUSD (Timeframe: PERIOD_H1)
RSI Value: 72.40
```
```
RSI in Standard Oversold Zone detected on EURUSD (Timeframe: PERIOD_H1)
RSI Value: 28.15
```

**Exhaustion zone alerts:**
```
RSI in Extreme Overbought Zone detected on EURUSD (Timeframe: PERIOD_H1)
RSI Value: 87.60
```
```
RSI in Extreme Oversold Zone detected on EURUSD (Timeframe: PERIOD_H1)
RSI Value: 12.30
```

All alerts are logged to the MT4 **Experts journal**.

---

## Two-Tier Zone Reference

```
RSI Scale:
  0 ──── 15 ──────────── 30 ─────────────── 70 ──────────── 85 ──── 100
         │                │                  │                │
    [EXHAUSTION       [OVERSOLD]         [OVERBOUGHT]   [EXHAUSTION
     LOWER=15]        [LOWER=30]         [UPPER=70]      UPPER=85]
```

| Zone | RSI Range | Alert Level | Typical Response |
|---|---|---|---|
| Extreme Oversold | < 15 | 🔴 High priority | Strongest reversal probability; look for confirmation |
| Standard Oversold | 15–30 | 🟡 Moderate | Early warning; watch for rejection candles |
| Neutral | 30–70 | — | No alert |
| Standard Overbought | 70–85 | 🟡 Moderate | Early warning; watch for reversal signals |
| Extreme Overbought | > 85 | 🔴 High priority | Strongest reversal probability; look for confirmation |

---

## Customization Guide

**For trending markets (reduce false signals):**

```
OverboughtLevel  = 80
OversoldLevel    = 20
ExhaustionUpper  = 90
ExhaustionLower  = 10
```

**For ranging/choppy markets (more responsive):**

```
OverboughtLevel  = 65
OversoldLevel    = 35
ExhaustionUpper  = 78
ExhaustionLower  = 22
```

**For scalping (M5/M15):**

```
RSIPeriod = 7 or 9
OverboughtLevel = 70
ExhaustionUpper = 82
```

---

## Installation

1. Copy `RSI_Exhaustion_Bands_Alert_001.mq4` to:
   ```
   MetaTrader 4/MQL4/Scripts/
   ```
2. Restart MT4 or right-click **Navigator** → **Refresh**
3. Drag onto a chart
4. Configure thresholds and click **OK**

---

## Requirements

- MetaTrader 4 (Build 600+)
- `#property strict` compliance (enforced)
- At least `RSIPeriod + 1` bars of history loaded
- MT4 must remain running for continuous monitoring

---

## Disclaimer

This script is provided for **educational and informational purposes only**. Extreme RSI readings in strong trends can persist for extended periods without reversing. Always confirm exhaustion signals with price structure analysis. Test on a demo account before live use.

---

## License

MIT License — free to use, modify, and distribute with attribution.
