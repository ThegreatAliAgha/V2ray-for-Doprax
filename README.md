# Accumulation Manipulation Distribution (AMD Model) Indicator

A TradingView Pine Script v5 indicator designed to identify price compression zones (Accumulation), detect false breakouts (Manipulation), and confirm real breakouts (Distribution).

## 📋 Overview

This indicator implements the AMD (Accumulation-Manipulation-Distribution) trading model, which analyzes price-volume behavior to identify:

1. **Accumulation (ABX)** - Tight price ranges with low volatility but high volume
2. **Manipulation** - False breakouts that return inside the range (liquidity grabs)
3. **Distribution** - Confirmed real breakouts that continue in the breakout direction

## 🎯 Key Features

- **Non-Repainting**: Uses confirmed closes to ensure signals don't disappear
- **Real-Time Compatible**: Works on live charts without recalculating
- **Visual Clarity**: Yellow boxes for accumulation, orange labels for manipulation, red labels for distribution
- **POC Calculation**: Displays Point of Control (volume-weighted average price) within accumulation zones
- **Customizable Parameters**: All key parameters are adjustable via inputs
- **Alert Support**: Built-in alerts for manipulation and distribution events
- **Multi-Timeframe**: Works on any timeframe, optimized for 1m-5m on NASDAQ

## 📊 How It Works

### 1. Accumulation Detection

The indicator scans for price compression zones based on:

- **Minimum Bar Count**: At least 10 candles (configurable)
- **ATR Compression**: Current ATR ≤ 50% of historical ATR
- **Tight Range**: Price range ≤ 0.25% of mid-price
- **Small Body Candles**: At least 60% of candles have small bodies (low volatility)
- **High Volume**: Average volume ≥ 1.0× the 20-bar average
- **Boundary Touches**: At least 2 touches on both top and bottom boundaries

When all conditions are met, a **yellow box** is drawn over the accumulation zone, and a **red dashed line** shows the POC (Point of Control).

### 2. Manipulation Detection

Occurs when:
- Price breaks above or below the box (high > top OR low < bottom)
- But close returns inside the box
- With higher than average volume

An **orange "Manipulation" label** appears at the fake breakout candle.

### 3. Distribution Detection

Confirmed when:
- Price closes outside the box (close > top OR close < bottom)
- Followed by 2 more consecutive closes in the same direction
- Total of 3 consecutive closes outside the box

A **red "Distribution" label** appears when confirmed, along with a small triangle indicating the breakout direction.

## 🎮 How to Use

### Installation

1. Open TradingView
2. Click on "Pine Editor" at the bottom of the screen
3. Copy the entire contents of `amd_model_indicator.pine`
4. Paste into the Pine Editor
5. Click "Add to Chart"

### Recommended Settings

- **Timeframe**: 1m, 3m, or 5m for intraday trading
- **Instruments**: Works best on liquid instruments (NASDAQ, SPY, EUR/USD, etc.)
- **Session**: Compatible with both regular and extended market sessions

### Input Parameters

#### Accumulation Settings
- **ATR Compression Multiplier** (0.5): Lower values require tighter compression
- **Minimum Bar Count in Box** (10): Minimum candles needed to form an accumulation zone
- **Max Range % of Price** (0.25): Maximum price range as a percentage
- **Volume Threshold Multiplier** (1.0): Minimum volume relative to average
- **Min % Small Body Candles** (60.0): Percentage of candles with small bodies
- **Min Touches per Boundary** (2): Minimum touches on top and bottom

#### Visual Settings
- **Show Manipulation Labels**: Toggle manipulation detection labels
- **Show Distribution Labels**: Toggle distribution detection labels
- **Show POC Line**: Toggle Point of Control line
- **Show Accumulation Boxes**: Toggle yellow accumulation boxes

#### Alert Settings
- **Enable Alerts**: Toggle alert notifications

## 📈 Trading Strategy

### Entry Signals

1. **Distribution Confirmation**: Enter when the red "Distribution" label appears
   - **Bullish**: Enter long when distribution confirms above the box
   - **Bearish**: Enter short when distribution confirms below the box

2. **Avoid Manipulation**: Do NOT enter on orange "Manipulation" labels
   - These are false breakouts designed to trap traders

### Stop Loss Placement

- **Conservative**: Place stop loss at the opposite side of the accumulation box
- **Aggressive**: Place stop loss just inside the breakout side of the box

### Take Profit

- Use a risk-reward ratio of at least 1:2
- Look for next major support/resistance levels
- Trail stop loss as price moves in your favor

### Example Trade Flow

```
1. Yellow box forms → Wait
2. Orange "Manipulation" label appears → Do NOT enter
3. Price returns to box → Wait
4. Red "Distribution" label appears → Enter trade
5. Set stop loss at box boundary
6. Target 2-3x the box height
```

## ⚠️ Important Notes

### Non-Repainting Guarantee

- All signals use **confirmed closes** only
- Distribution requires 3 consecutive closes for confirmation
- Once a label appears, it will NOT disappear (unless you reload the chart and it hasn't re-confirmed)

### Performance Optimization

- Designed for lightweight computation
- Supports up to 500 boxes, labels, and lines
- Automatically removes old boxes after 100 bars of inactivity

### Limitations

- Not suitable for very slow-moving markets with no volatility
- May produce fewer signals in ranging markets
- Works best in markets with clear accumulation-distribution cycles
- Requires sufficient historical data (at least 50 bars) for ATR calculation

## 🔧 Troubleshooting

### No Boxes Appearing

- Check if your timeframe has sufficient volatility
- Try lowering the **ATR Compression Multiplier** to 0.3-0.4
- Reduce **Minimum Bar Count** to 7-8
- Increase **Max Range %** to 0.3-0.5

### Too Many False Signals

- Increase **Minimum Bar Count** to 15-20
- Increase **Volume Threshold Multiplier** to 1.2-1.5
- Increase **Min Touches per Boundary** to 3-4

### Boxes Don't Close After Breakout

- This is normal behavior if the breakout hasn't been confirmed
- Distribution requires 3 consecutive closes outside the box
- If price re-enters the box, the breakout is invalidated

## 📚 Advanced Tips

1. **Combine with Other Indicators**: Use with RSI, MACD, or market profile for confluence
2. **Multi-Timeframe Analysis**: Check higher timeframes for overall trend direction
3. **Volume Confirmation**: Look for volume spikes on distribution candles
4. **News Events**: Be cautious during major news releases (can invalidate accumulation zones)
5. **Session Awareness**: Accumulation often forms during low-liquidity periods (pre-market, lunch)

## 🔄 Version History

- **v1.0** (2025-10-13): Initial release
  - Accumulation detection with multiple criteria
  - Manipulation detection (fake breakouts)
  - Distribution detection with 3-bar confirmation
  - POC calculation using VWAP
  - Full customization via inputs
  - Alert support

## 📄 License

This indicator is provided as-is for educational and trading purposes. Use at your own risk.

## 🤝 Support

For questions, issues, or suggestions:
- Review the code comments in `amd_model_indicator.pine`
- Adjust input parameters based on your specific market and timeframe
- Test on historical data before using in live trading

---

**Disclaimer**: This indicator is for educational purposes only. Past performance does not guarantee future results. Always use proper risk management and never risk more than you can afford to lose.
