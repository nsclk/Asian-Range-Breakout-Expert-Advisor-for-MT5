# Asian-Range-Breakout-Expert-Advisor-for-MT5

A robust, fully automated MetaTrader 5 Expert Advisor (EA) that trades **mean-reversion after Asian session range breakouts**. This EA is engineered for **live trading**, includes advanced risk management, automated notifications (Telegram, email, mobile), and comprehensive trade/statistics export features.

---

## 🚀 Overview

**Asian Range EA v2.03** is designed to:
- Identify the Asian session price range
- Detect breakout and reentry conditions for high-probability mean-reversion trades
- Enter and manage trades with dynamic stop loss (SL), take profit (TP), and position sizing
- Export daily trade and performance statistics
- Send live notifications to Telegram, email, and mobile devices
- Support automatic Daylight Saving Time (DST) adjustments
- Operate fully hands-off for both backtesting and live trading

---

## 🧠 How the Strategy Works

1. **Detect the Asian Range:**  
   During user-defined Asian session hours, the EA records the high and low prices to establish the "range."

2. **Validate the Range:**  
   At the end of the session, the EA checks if the range size is within configurable min/max pips.

3. **Breakout Detection:**  
   The EA monitors for a 5-minute candle close **outside** the range (either above or below).

4. **Reentry Setup:**  
   After a breakout, the EA waits for price to close **back inside** the range (mean reversion trigger).

5. **Trade Execution:**  
   On reentry, the EA:
   - Calculates stop loss as the extreme price between breakout and reentry
   - Sets take profit at a configurable multiple of the SL distance
   - Calculates lot size based on account risk % and SL distance
   - Sends a market order (long or short)

6. **Trade Management:**  
   The EA tracks the open trade, exports results, and sends notifications when closed (TP, SL, or breakeven).

7. **Daily Reset:**  
   At a set time, stats are exported and all variables are reset for the next trading day.

---

## ✨ Features

- **Automatic Asian session detection**
- **Mean-reversion entry after false breakouts**
- **Dynamic risk-based position sizing**
- **Configurable SL/TP logic (Risk:Reward)**
- **Automatic handling of Daylight Saving Time (DST)**
- **Telegram, email, and push notification support**
- **Trade, range, and performance CSV export**
- **Comprehensive live trading logs**
- **Daily reset and summary**

---

## 🛠 Requirements

- **MetaTrader 5 (MT5)**
- Compatible broker/account that supports algorithmic trading
- Internet access (for Telegram/email notifications)
- Optional: SMTP setup for email alerts
- Optional: Mobile app for push notifications

---

## 📝 Installation & Setup

1. **Download the EA Source Code**
    - Copy `AsianRangeEA_v2.03.mq5` to your `MQL5/Experts/` directory.

2. **Compile the EA**
    - Open MetaEditor, select the EA file, and click **Compile**.

3. **Attach to Chart**
    - Open MT5, attach the EA to the desired symbol (e.g., EURUSD) and timeframe (recommended: M5).

4. **Configure Inputs**
    - Set your preferred session times, risk management, and notification options via EA inputs.

5. **(Optional) Setup Telegram Notifications**
    - Create a bot with [@BotFather](https://t.me/botfather) on Telegram.
    - Add your `TelegramBotToken` and `TelegramChatID` in the EA inputs.
    - More info: [Telegram API Docs](https://core.telegram.org/bots)

6. **(Optional) Setup Email Alerts**
    - Configure email settings in MT5 (`Tools > Options > Email`).

7. **(Optional) Enable Mobile Push Notifications**
    - Configure in MT5 (`Tools > Options > Notifications`), link your MetaQuotes ID.

---

## ⚙️ Input Parameters

| Group                  | Name                       | Description                                      | Default      |
|------------------------|----------------------------|--------------------------------------------------|--------------|
| Time Settings (Auto DST)| EnableDSTAdjustment        | Auto-adjust Asian session for DST                | true         |
|                        | SummerAsianStartHour       | Summer session start hour (server time)          | 0            |
|                        | SummerAsianEndHour         | Summer session end hour (server time)            | 7            |
|                        | WinterAsianStartHour       | Winter session start hour                        | 1            |
|                        | WinterAsianEndHour         | Winter session end hour                          | 8            |
|                        | MaxBreakWaitHour           | Max hour to wait for breakout                    | 12           |
| Risk Management        | RiskPercentage             | % of balance risked per trade                    | 2.0          |
|                        | MaxSpreadPips              | Max spread allowed for trading                   | 3            |
|                        | TPMultiplier               | Take profit multiplier (TP = SL * X)             | 1.5          |
| Strategy Settings      | MinRangePips               | Minimum valid Asian range size                   | 10           |
|                        | MaxRangePips               | Maximum valid Asian range size                   | 100          |
|                        | AllowBothDirections        | Allow both long/short trades                     | true         |
| Display Settings       | ShowAsianRange             | Draw range lines on chart                        | true         |
|                        | RangeColor                 | Color of range lines                             | Yellow       |
| CSV Export Settings    | EnableCSVExport            | Export data to CSV                               | true         |
|                        | CSVFileName                | Name of CSV file                                 | AsianRangeEA_Data.csv |
| Live Trading           | EnableDetailedLogging      | Print detailed logs                              | true         |
|                        | EnableTickLogging          | Print every tick (resource intensive)            | false        |
|                        | MaxSlippagePips            | Max allowed slippage (pips)                      | 2            |
|                        | EnableEmailAlerts          | Send email alerts                                | false        |
|                        | EnableMobileAlerts         | Send push notifications                          | false        |
| Telegram Settings      | EnableTelegramAlerts       | Send Telegram notifications                      | true         |
|                        | TelegramBotToken           | Your Telegram bot token                          | (required)   |
|                        | TelegramChatID             | Your Telegram chat/group ID                      | (required)   |

---

## 📊 Data Export (CSV)

- All daily stats and trade data are exported (if enabled) to a CSV file for record keeping and analysis.
- **Fields include:** date, symbol, Asian range high/low, range size, break direction, entry/exit times, SL/TP/lot size, result, P&L, spread, comments, etc.

---

## 📱 Notifications

### Telegram
- Notifies for:
  - Range formation
  - Breakout detected
  - Trade executed
  - TP/SL hit
  - Daily summary
  - EA startup/shutdown

### Email & Mobile
- (Optional) notifications for all major events.

---

## 🧮 Example Trade Flow

1. **EA starts** and detects Asian session start.
2. **Records high/low** during session.
3. **Session ends:** Range validated (e.g., 22 pips).
4. **Breakout:** Price closes above range (upward breakout).
5. **Reentry:** Price closes back inside range.  
   → EA prepares SHORT trade:
   - **Stop Loss:** Set to highest price during breakout/reentry
   - **Take Profit:** 1.5x SL distance
   - **Lot Size:** Based on risk percent and SL distance
   - **Market Sell** order is executed
6. **Trade is managed:** If TP/SL is hit, result is recorded.
7. **End of day:** Stats exported, notifications sent, all values reset.

---

## 🛡️ Risk Management

- Trades are sized so **each trade risks exactly the % of account** specified, no more.
- Trades are skipped if:
  - Spread exceeds user-set maximum
  - Invalid range
  - Trading is disabled or market is closed

---

## 📝 Logging & Debugging

- All major events and errors are logged in the MT5 **Experts** tab.
- Detailed logs can be enabled for full traceability.
- Notification failures, trade errors, and other issues are clearly reported.

---

## 📚 Example Output (Logs/Notifications)

=== EXECUTING SHORT TRADE ===
Server Time: 2025.07.21 09:16:05
Entry Price: 1.27563
Stop Loss: 1.27693
Take Profit: 1.27368
SL Distance: 13.0 pips
TP Distance: 29.5 pips
Risk:Reward = 1:1.50
Risk Amount: $20.00
Calculated Lot Size: 0.12
Trade Request Sent...
Result Code: 10009
Deal: 123456789
Order: 987654321
Volume: 0.12
Price: 1.27563
Comment: Asian Range SHORT v2.03
✅ SHORT TRADE SUCCESSFULLY EXECUTED!

---

## Telegram Example

🤖 Asian Range EA v2.03
📈 Symbol: EURUSD
🕐 Time: 2025.07.21 09:16
💰 Balance: $1020.36

✅ İŞLEM AÇILDI
📊 Direction: SHORT
💰 Entry Price: 1.27563
🛑 Stop Loss: 1.27693
🎯 Take Profit: 1.27368
📦 Lot Size: 0.12
💵 Risk Amount: $20.00

---

## ⚠️ Disclaimer

**Trading forex and CFDs is risky!**  
This EA is provided for educational and research purposes only.  
You are solely responsible for any trades placed, losses, or legal requirements.  
**Always test with a demo account before live deployment!**

---

## 🙌 Contributions

Pull requests, feature suggestions, and bug reports are welcome.  
Open an issue or submit a PR for improvements!

---

## 📄 License

MIT License (see `LICENSE` file for details).

---

## ✉️ Contact

- [Your GitHub profile link]
- Telegram: [Your username or bot]
- Email: [Your contact email]

---

**Enjoy safer and smarter live trading with Asian Range EA v2.03!**


