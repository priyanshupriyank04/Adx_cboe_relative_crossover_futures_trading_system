# CBOE + ADX Relative Crossover Futures Trading System

This trading system scans **all current month NSE stock futures** and generates **bullish** and **bearish** trading signals based on **relative crossovers** between the **CBOE oscillator** and **ADX (Directional Index)** indicators.

---

## How It Works

1. **Fetches Current Month Stock Futures**  
   - Excludes NIFTY and BANKNIFTY.  
   - Uses the Kite API to get the active contracts.

2. **Updates OHLC Data**  
   - Fetches the last **50 trading days** of daily OHLC (Open, High, Low, Close, Volume) data for each contract.  
   - Optionally adds a **synthetic daily candle** for the current day (aggregated from hourly data).

3. **Calculates Indicators**  
   - **CBOE-based oscillator:** Computes odds of **bull**, **bear**, and **stagnant** market conditions.  
   - **ADX (Wilder’s smoothing):** Includes `DI+` and `DI−` values.

4. **Checks for Relative Crossovers**  
   - Crossovers are checked on **yesterday (-1)** and **today (0)** using the last **3 trading days**.  
   - **Bullish Trigger**:  
     - `odd_bull` crosses above `odd_bear` (on yesterday or today), **AND**  
     - `di_plus` crosses above `di_minus` (on yesterday or today).  
   - **Bearish Trigger**:  
     - `odd_bear` crosses above `odd_bull` (on yesterday or today), **AND**  
     - `di_minus` crosses above `di_plus` (on yesterday or today).  
   - Crossovers for **CBOE** and **ADX** can occur on **different days** within the last 2 sessions.

5. **Outputs Results**  
   - **Bullish signals** → `bull_relative.csv`  
   - **Bearish signals** → `bear_relative.csv`

---

## How to Run

1. Configure your **Kite API credentials** in the script.
2. Ensure **PostgreSQL** is running (tables will be created automatically if missing).
3. Run the system:
   ```bash
   python main.py
