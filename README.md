# Financial Scenario Simulation Framework

This notebook re-implements (with simplifications) the framework from Abdymomunov et al. (2024) using simulated data.

## Overview

This code implements a sophisticated scenario generation methodology that:
- Models primary financial factors using realistic dynamics (Ornstein-Uhlenbeck processes)
- Simulates secondary factors using quantile regression techniques
- Constructs a t-copula for dependency structure among remaining factors
- Generates joint scenarios and calculates P&L impacts
- Identifies material risk factors and clusters tail-loss scenarios

The approach is particularly valuable for stress testing, risk management, and understanding vulnerabilities during extreme market conditions.

## Key Methodologies

### 1. Factor Modeling
- **Primary factors**: Simulated using Ornstein-Uhlenbeck processes with realistic mean-reversion properties for interest rates
- **Secondary factors**: Modeled using:
  - Standard quantile regression (Equation 1)
  - Quantile autoregression for factors with strong autocorrelation (Equation 2)
- **Remaining factors**: Modeled using GARCH-filtered returns with t-copula dependency structure

### 2. Scenario Generation
- Rolling-window historical simulation approach
- Implementation of Equation 3 for calculating tau based on historical severity
- Filtering of implausible scenarios (e.g., negative interest rates)

### 3. Risk Analysis
- P&L calculation incorporating DV01 and convexity effects
- Material factor identification (~70% coverage threshold)
- K-means clustering of tail-loss scenarios
- Silhouette analysis for optimal cluster determination

## Dependencies

- Python 3.7+
- NumPy
- Pandas
- Matplotlib
- Seaborn
- scikit-learn
- arch (for GARCH modeling)
- SciPy

## How to Run

1. Install required dependencies:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn arch scipy
   ```

2. Execute the script:
   
   Run the notebook MarketShockScenarios.ipynb
 

## Output Interpretation

The code produces several key outputs:

1. **Primary Factor Shocks**: Historical 3-month changes for core financial factors, including:
   - SP500 (equity index)
   - BAA_AAA (credit spread)
   - UST10, TERM, UST3 (interest rate factors)
   - USD_EUR (FX rate)
   - Commodity factors (ENERGY, METAL, GOLD)

2. **PnL Distribution Analysis**:
   - Statistics of simulated P&L distributions for two firms
   - Visualization of P&L distributions with 1% tail thresholds

3. **Material Risk Factors**:
   - Identification of factors contributing to ~70% of portfolio risk
   - Cumulative coverage metrics

4. **Tail-Loss Scenario Clustering**:
   - Representative scenarios from identified clusters
   - Silhouette analysis for optimal cluster count
   - Visualizations of clustered tail scenarios

5. **Diagnostic Plots**:
   - Primary factor shocks over time
   - Histograms of secondary factor shocks
   - Comparative bar plots of scenario P&Ls vs. tail percentiles

## Key Functions

- `simulate_ou()`: Simulates Ornstein-Uhlenbeck process for interest rate factors
- `fix_corr_matrix()`: Ensures correlation matrices are positive semi-definite
- `get_tau()`: Calculates tau based on historical severity of shock (Equation 3)
- `quantile_predict_shocks()`: Implements standard quantile regression (Equation 1)
- `quantile_autoreg_predict()`: Implements quantile autoregression (Equation 2)
- `safe_garch_fit()`: Applies GARCH filtering for marginal distributions
- `filter_implausible_scenarios()`: Filters out unrealistic scenarios (e.g., negative rates)

## References

The implementation follows methodologies similar to those described in the "Designing Market Shock Scenarios" paper.
