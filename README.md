# Options Analytics & Backtesting Engine

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Django](https://img.shields.io/badge/Django-Backend-green)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-orange)
![License](https://img.shields.io/github/license/Garvit423/options-fiesta)

**Options Analytics Engine** is a comprehensive, web-based quantitative dashboard designed for modeling derivatives, calculating Implied Volatility (IV), computing the Greeks, and backtesting delta-neutral trading strategies. 

Built with a Django backend and a dynamic frontend, the engine ingests raw options chain CSV data, processes it via memory-efficient Pandas pipelines, and serves the analytics through RESTful endpoints.

## Key Features

* **Implied Volatility (IV) Modeling:** * Solves for volatility (σ) using the Black-Scholes pricing model.
    * Utilizes SciPy's numerical root-finding algorithms (Brent's method) to match observed market premiums to theoretical model prices.
    * Generates interactive, time-series IV plots filtered by date ranges and option types.
* **Dynamic Options Greeks Calculator:**
    * Computes first and second-order partial derivatives (Δ, Γ, Θ, Vega, ρ) directly from the Black-Scholes equation.
    * Implements server-side caching (`django.core.cache`) to optimize repeated complex mathematical operations and improve dashboard rendering speeds.
* **Algorithmic Backtesting Module:**
    * Evaluates the historical performance of multi-leg strategies including **Straddles**, **Strangle**, and **Butterfly Spreads**.
    * Calculates and visualizes quantitative metrics: Max Drawdown, Annualized Sharpe Ratio, Win Rate, and Equity Curves.

## System Architecture

The application follows a modular, decoupled architecture optimized for quantitative analysis rather than traditional CRUD operations:
1.  **Data Ingestion Layer:** Reads tick/minute-level options data from static CSV files, eliminating database latency for heavy sequential reads.
2.  **Controller & Processing (Django Views):** Acts as the API gateway, routing requests to dedicated Python quant modules (`greeks.py`, `iv.py`, `backtest.py`).
3.  **Algorithmic Engine:** Uses NumPy and Pandas to filter market hours, merge spot prices with derivative prices, and execute vectorized calculations.
4.  **Presentation Layer:** Asynchronously fetches JSON payloads from the backend to dynamically render data visualizations via Bootstrap and JavaScript without full page reloads.

## Backtest Performance Summary (Example)

| Strategy  | Trades | Win Rate | Total P&L (₹) | Max Drawdown | Ann. Sharpe Ratio |
|-----------|--------|----------|--------------:|-------------:|------------------:|
| Straddle  | 11     | 72.73%   | 8,372.50      | -2.45%       | 121.79            |
| Butterfly | 5      | 80.00%   | 46,000.00     | -0.34%       | 116.04            |
| Strangle  | 9      | 66.00%   | 23,018.00     | -1.84%       | 119.64            |

*(Note: Results are based on historical Nifty 50 data and are for educational purposes, not financial advice.)*

## Local Installation & Setup

### Prerequisites
* Python 3.9+
* Git

### Step-by-Step Guide
1. **Clone the repository:**
   ```bash
   git clone [https://github.com/Garvit423/options-fiesta.git](https://github.com/Garvit423/options-fiesta.git)
   cd options-fiesta
   ```

2. **Create and activate a virtual environment:**
   ```bash
   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   
   # Windows
   python -m venv venv
   venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Navigate to the core application directory:**
   ```bash
   cd options_dashboard
   ```

5. **Start the development server:**
   ```bash
   python manage.py runserver
   ```
6. **Access the dashboard:** Open your browser and navigate to `http://127.0.0.1:8000/`.

## Troubleshooting

**Error:** `FileNotFoundError: [Errno 2] No such file or directory: './12Dec-Nifty'`  
**Fix:** The analytics engine requires local data files to run. Ensure that the `12Dec-Nifty` data folder and the `nifty_underlying.csv` file are located directly inside the inner `options_dashboard` directory (at the same level as `manage.py`).

<!-- ## License
This project is licensed under the terms in the [LICENSE](LICENSE) file. -->