# Auto-Research System Specification
## Hyperparameter Optimization for Crypto Trading Algorithm

**Version:** 1.0  
**Date:** March 16, 2026  
**Purpose:** Optimize crypto trading algorithm parameters  
**Target Metrics:** CAGR, Sharpe Ratio, Max Drawdown  
**Status:** Ready for Implementation

---

## Executive Summary

### Research Goals

**Primary Objective:** Find optimal hyperparameters for crypto trading algorithm

**Optimization Targets:**
1. **Maximize CAGR** (Compound Annual Growth Rate)
2. **Maximize Sharpe Ratio** (risk-adjusted returns)
3. **Minimize Max Drawdown** (risk control)
4. **Maximize Calmar Ratio** (CAGR / Max DD)

**Search Space:** 50+ hyperparameters across signal generation, risk management, execution

**Method:** Bayesian Optimization + Grid Search + Walk-Forward Analysis

**Expected Timeline:** 7-14 days (automated)

---

## Hyperparameter Search Space

### 1. Signal Generation Parameters (20 parameters)

#### Fundamental/On-Chain Weights
```python
onchain_params = {
    'exchange_flow_weight': Uniform(0.1, 0.5),
    'whale_activity_weight': Uniform(0.1, 0.4),
    'active_addresses_weight': Uniform(0.05, 0.3),
    'hash_rate_weight': Uniform(0.05, 0.2),  # BTC only
    'futures_oi_weight': Uniform(0.05, 0.2),
    
    # Scoring thresholds
    'whale_threshold_usd': LogUniform(1e5, 1e7),  # $100K - $10M
    'exchange_flow_threshold': LogUniform(1e3, 1e5),  # BTC units
    'address_growth_threshold': Uniform(0.1, 0.5),  # 10-50% MoM
}
```

#### Social Sentiment Parameters
```python
social_params = {
    'influencer_weight': Uniform(0.4, 0.8),
    'retail_weight': Uniform(0.2, 0.6),
    'reddit_weight': Uniform(0.1, 0.3),
    'telegram_weight': Uniform(0.05, 0.2),
    
    # Viral detection
    'viral_multiplier': Uniform(2.0, 10.0),
    'viral_threshold': Uniform(3.0, 8.0),  # Volume ratio
    'fud_penalty': Uniform(0.1, 0.5),
    
    # Time decay
    'sentiment_decay_hours': Uniform(12, 72),
}
```

#### Technical Analysis Parameters
```python
technical_params = {
    # LSTM-GNN architecture
    'lstm_hidden_size': Choice([64, 128, 256]),
    'gnn_hidden_size': Choice([32, 64, 128]),
    'lstm_layers': Choice([1, 2, 3]),
    'dropout_rate': Uniform(0.1, 0.5),
    
    # Sequence length
    'sequence_length': Choice([30, 60, 90]),  # days
    
    # BTC correlation
    'btc_correlation_window': Choice([7, 14, 30]),  # days
    'btc_correlation_weight': Uniform(0.2, 0.6),
    
    # Prediction threshold
    'entry_probability_threshold': Uniform(0.55, 0.75),
    'exit_probability_threshold': Uniform(0.45, 0.65),
}
```

#### Funding Rate Parameters
```python
funding_params = {
    'funding_signal_weight': Uniform(0.1, 0.3),
    'carry_trade_threshold': Uniform(0.15, 0.50),  # 15-50% APY
    'funding_arb_threshold': Uniform(0.20, 0.60),  # 20-60% APY
    'funding_lookback_hours': Choice([8, 24, 72]),
}
```

---

### 2. Signal Fusion Parameters (5 parameters)

```python
fusion_params = {
    # Agent weights
    'onchain_weight': Uniform(0.15, 0.35),
    'social_weight': Uniform(0.25, 0.45),
    'technical_weight': Uniform(0.15, 0.35),
    'funding_weight': Uniform(0.05, 0.20),
    
    # Confidence weighting
    'confidence_power': Uniform(0.5, 2.0),  # Exponent for confidence weighting
    
    # Regime adaptation
    'regime_adaptation_speed': Uniform(0.1, 0.5),  # How fast weights change
}
```

**Constraint:** Weights must sum to 1.0

---

### 3. Risk Management Parameters (15 parameters)

#### Volatility & VaR
```python
risk_params = {
    # GARCH-GRU
    'garch_p': Choice([1, 2]),
    'garch_q': Choice([1, 2]),
    'gru_hidden_size': Choice([32, 64, 128]),
    
    # VaR calculation
    'var_confidence': Choice([0.95, 0.975, 0.99]),
    'var_lookback_days': Choice([30, 60, 90, 180]),
    'var_multiplier': Uniform(1.0, 2.0),  # Safety factor
    
    # Regime thresholds
    'crisis_vol_threshold': Uniform(0.8, 1.5),  # 80-150% annual vol
    'elevated_vol_threshold': Uniform(0.5, 0.9),
    'calm_vol_threshold': Uniform(0.2, 0.4),
}
```

#### Position Sizing
```python
position_params = {
    # Kelly criterion
    'kelly_fraction': Uniform(0.25, 0.75),  # Fraction of full Kelly
    'kelly_cap': Uniform(0.05, 0.15),  # Max position (5-15%)
    
    # Leverage
    'max_leverage_calm': Uniform(3.0, 7.0),
    'max_leverage_normal': Uniform(2.0, 4.0),
    'max_leverage_elevated': Uniform(1.0, 2.0),
    'max_leverage_crisis': Uniform(0.5, 1.0),
    
    # Concentration limits
    'max_single_position': Uniform(0.03, 0.08),  # 3-8%
    'max_sector_exposure': Uniform(0.20, 0.40),  # 20-40%
    'max_btc_correlation': Uniform(0.6, 0.85),  # Max correlation between positions
}
```

#### Circuit Breakers
```python
circuit_breaker_params = {
    'daily_loss_limit': Uniform(0.05, 0.12),  # 5-12%
    'max_drawdown': Uniform(0.10, 0.20),  # 10-20%
    'var_breach_limit': Choice([2, 3, 4, 5]),  # Breaches per day
    'consecutive_losses': Choice([3, 4, 5, 6]),
    'liquidation_buffer': Uniform(0.05, 0.15),  # 5-15% from liquidation
}
```

---

### 4. Execution Parameters (10 parameters)

```python
execution_params = {
    # VWAP slicing
    'num_slices': Choice([5, 7, 10, 15]),
    'execution_horizon_hours': Uniform(2, 8),
    'limit_order_discount_bps': Uniform(5, 20),  # 5-20 bps
    'max_market_order_pct': Uniform(0.1, 0.3),  # 10-30%
    
    # Exchange allocation
    'binance_allocation': Uniform(0.25, 0.40),
    'coinbase_allocation': Uniform(0.20, 0.35),
    'kraken_allocation': Uniform(0.15, 0.30),
    'dex_allocation': Uniform(0.05, 0.15),
    
    # Slippage tolerance
    'max_slippage_bps': Uniform(30, 100),  # 30-100 bps
    'fee_optimization': Choice(['maker', 'taker', 'mixed']),
}
```

---

## Optimization Strategy

### Multi-Objective Optimization

**Objective Function:**
```python
def objective(params):
    # Run backtest with params
    results = backtest(params, data=train_data)
    
    # Calculate metrics
    cagr = results['cagr']
    sharpe = results['sharpe_ratio']
    max_dd = results['max_drawdown']
    calmar = cagr / abs(max_dd) if max_dd != 0 else 0
    
    # Multi-objective score (weighted)
    score = (
        0.40 * normalize(cagr, 0, 2.0) +      # 40% CAGR
        0.35 * normalize(sharpe, 0, 3.0) +    # 35% Sharpe
        0.15 * normalize(-max_dd, -0.5, 0) +  # 15% (minimize DD)
        0.10 * normalize(calmar, 0, 3.0)      # 10% Calmar
    )
    
    return score
```

**Constraints:**
- Max drawdown < 25% (hard constraint)
- Sharpe ratio > 0.5 (minimum acceptable)
- CAGR > 10% (minimum acceptable)
- Win rate > 40% (minimum acceptable)

---

### Optimization Algorithm: Bayesian Optimization + Grid Search

```python
class CryptoHyperparameterOptimizer:
    def __init__(self, search_space, data):
        self.search_space = search_space
        self.data = data
        self.optimizer = None
        
    def optimize(self, n_iterations=500):
        """
        Two-phase optimization:
        1. Bayesian Optimization (exploration)
        2. Grid Search (refinement around best)
        """
        # Phase 1: Bayesian Optimization (300 iterations)
        print("Phase 1: Bayesian Optimization...")
        self.optimizer = BayesianOptimization(
            f=self.objective,
            search_space=self.search_space,
            acquisition_function='EI',  # Expected Improvement
            n_initial_points=50,
            n_iterations=300
        )
        
        best_params, best_score = self.optimizer.optimize()
        
        # Phase 2: Grid Search around best (200 iterations)
        print("Phase 2: Local Grid Search...")
        refined_space = self.create_local_grid(best_params, radius=0.1)
        grid_results = self.grid_search(refined_space, n_points=200)
        
        # Get final best
        final_best = max(grid_results, key=lambda x: x['score'])
        
        return final_best['params'], final_best['score']
    
    def objective(self, params):
        """Evaluate parameter set."""
        # Walk-forward validation
        scores = []
        
        for train_idx, val_idx in self.walk_forward_splits():
            train_data = self.data.iloc[train_idx]
            val_data = self.data.iloc[val_idx]
            
            # Train models (LSTM-GNN, GARCH-GRU)
            models = self.train_models(params, train_data)
            
            # Backtest
            results = self.backtest(params, models, val_data)
            
            # Calculate score
            score = self.calculate_score(results)
            scores.append(score)
        
        # Return average (robust to overfitting)
        return np.mean(scores)
    
    def walk_forward_splits(self, train_days=180, val_days=30, step_days=30):
        """
        Walk-forward validation splits.
        """
        splits = []
        start = 0
        
        while start + train_days + val_days <= len(self.data):
            train_end = start + train_days
            val_end = train_end + val_days
            
            splits.append((
                range(start, train_end),
                range(train_end, val_end)
            ))
            
            start += step_days
        
        return splits
```

---

## Backtesting Engine

### Crypto-Specific Backtester

```python
class CryptoBacktester:
    def __init__(self, params, models, data):
        self.params = params
        self.models = models
        self.data = data
        self.initial_capital = 1_000_000
        self.capital = self.initial_capital
        
    def run(self) -> BacktestResults:
        """
        Run full backtest.
        """
        # Initialize
        portfolio = Portfolio()
        trades = []
        daily_returns = []
        
        # Iterate through time
        for timestamp in self.data.index:
            # Get market data
            market_data = self.get_market_data(timestamp)
            
            # Generate signals
            signals = self.generate_signals(market_data)
            
            # Risk management
            risk_metrics = self.assess_risk(market_data)
            
            # Check circuit breakers
            if self.check_circuit_breakers(portfolio, risk_metrics):
                self.close_all_positions(portfolio, timestamp)
                continue
            
            # Generate trades
            new_trades = self.generate_trades(signals, risk_metrics, portfolio)
            
            # Execute trades
            for trade in new_trades:
                execution = self.execute_trade(trade, market_data)
                trades.append(execution)
                portfolio.update(execution)
            
            # Update portfolio value
            portfolio.update_values(market_data)
            
            # Record daily return
            daily_return = portfolio.calculate_daily_return()
            daily_returns.append(daily_return)
        
        # Calculate metrics
        metrics = self.calculate_metrics(daily_returns, trades, portfolio)
        
        return BacktestResults(
            params=self.params,
            metrics=metrics,
            trades=trades,
            daily_returns=daily_returns,
            equity_curve=portfolio.equity_curve
        )
```

### Performance Metrics Calculation

```python
def calculate_metrics(self, daily_returns, trades, portfolio) -> dict:
    """
    Calculate comprehensive performance metrics.
    """
    daily_returns = np.array(daily_returns)
    
    # Returns
    total_return = (portfolio.value / self.initial_capital) - 1
    days = len(daily_returns)
    cagr = (1 + total_return) ** (365 / days) - 1
    
    # Risk-adjusted
    sharpe = np.sqrt(252) * daily_returns.mean() / daily_returns.std()
    downside_returns = daily_returns[daily_returns < 0]
    sortino = np.sqrt(252) * daily_returns.mean() / downside_returns.std() if len(downside_returns) > 0 else 0
    
    # Drawdown
    equity_curve = np.cumprod(1 + daily_returns)
    running_max = np.maximum.accumulate(equity_curve)
    drawdown = (equity_curve - running_max) / running_max
    max_drawdown = drawdown.min()
    
    # Calmar ratio
    calmar = cagr / abs(max_drawdown) if max_drawdown != 0 else 0
    
    # Trading metrics
    winning_trades = [t for t in trades if t.pnl > 0]
    losing_trades = [t for t in trades if t.pnl < 0]
    win_rate = len(winning_trades) / len(trades) if trades else 0
    avg_win = np.mean([t.pnl for t in winning_trades]) if winning_trades else 0
    avg_loss = np.mean([t.pnl for t in losing_trades]) if losing_trades else 0
    profit_factor = abs(sum(t.pnl for t in winning_trades) / sum(t.pnl for t in losing_trades)) if losing_trades else float('inf')
    
    # Costs
    total_slippage = sum(t.slippage for t in trades)
    total_fees = sum(t.fee for t in trades)
    avg_slippage_bps = total_slippage / len(trades) * 10000 if trades else 0
    
    # Crypto-specific
    funding_income = sum(t.funding_payment for t in trades if hasattr(t, 'funding_payment'))
    staking_yield = portfolio.calculate_staking_yield()
    
    return {
        'cagr': cagr,
        'total_return': total_return,
        'sharpe_ratio': sharpe,
        'sortino_ratio': sortino,
        'max_drawdown': max_drawdown,
        'calmar_ratio': calmar,
        'win_rate': win_rate,
        'profit_factor': profit_factor,
        'avg_win': avg_win,
        'avg_loss': avg_loss,
        'total_trades': len(trades),
        'avg_slippage_bps': avg_slippage_bps,
        'total_fees': total_fees,
        'funding_income': funding_income,
        'staking_yield': staking_yield,
    }
```

---

## Research Experiments

### Experiment 1: Baseline Optimization

**Goal:** Find optimal parameters for base crypto algorithm

**Search Space:** All 50 parameters

**Data:** BTC, ETH, Top 20 altcoins (2020-2026)

**Timeline:** 7 days (parallel on 10 GPUs)

**Expected Output:**
- Optimal parameter set
- Performance metrics (CAGR, Sharpe, Max DD)
- Equity curve
- Trade log

---

### Experiment 2: Regime-Specific Optimization

**Goal:** Different parameters for different market regimes

**Regimes:**
1. **Bull Market** (BTC up >50% YoY)
2. **Bear Market** (BTC down >30% YoY)
3. **Sideways** (BTC within ±20%)
4. **High Vol** (BTC vol >80%)
5. **Low Vol** (BTC vol <40%)

**Approach:**
- Label each day with regime
- Optimize separate parameters for each regime
- Build regime classifier (random forest)

**Expected Output:**
- 5 parameter sets (one per regime)
- Regime classification accuracy
- Performance by regime

---

### Experiment 3: Asset-Specific Optimization

**Goal:** Optimize parameters per asset class

**Asset Classes:**
1. **Large Cap** (BTC, ETH)
2. **Mid Cap** (Top 10-50)
3. **Small Cap** (Top 50-200)
4. **DeFi Tokens**
5. **Layer 1 Tokens**

**Expected Output:**
- Parameter sets per asset class
- Performance comparison
- Cross-asset robustness test

---

### Experiment 4: Robustness Testing

**Goal:** Test parameter robustness across:
- Different time periods
- Different exchanges
- Different market conditions

**Tests:**
1. **Out-of-Sample:** Test on 2026 data (not in optimization)
2. **Cross-Exchange:** Train on Binance, test on Coinbase/Kraken
3. **Monte Carlo:** Random perturbations to parameters
4. **Transaction Cost Sensitivity:** Vary fees from 0-0.2%

**Expected Output:**
- Robustness scores
- Parameter stability analysis
- Recommended parameter ranges

---

## Implementation Plan

### Phase 1: Infrastructure Setup (Week 1)

**Tasks:**
- [ ] Setup backtesting engine
- [ ] Integrate crypto data (Binance, Coinbase, on-chain)
- [ ] Implement parameter search space
- [ ] Setup Bayesian Optimization (Optuna or Hyperopt)
- [ ] Configure GPU cluster (10x A10G)

**Deliverable:** Working optimization pipeline

---

### Phase 2: Baseline Optimization (Week 2)

**Tasks:**
- [ ] Run Experiment 1 (baseline)
- [ ] Monitor optimization progress
- [ ] Analyze initial results
- [ ] Tune acquisition function if needed

**Deliverable:** Optimal baseline parameters

---

### Phase 3: Advanced Experiments (Week 3)

**Tasks:**
- [ ] Run Experiment 2 (regime-specific)
- [ ] Run Experiment 3 (asset-specific)
- [ ] Run Experiment 4 (robustness)
- [ ] Cross-validate results

**Deliverable:** Comprehensive parameter sets

---

### Phase 4: Validation & Reporting (Week 4)

**Tasks:**
- [ ] Out-of-sample testing (2026 data)
- [ ] Paper trading with optimal params
- [ ] Generate research report
- [ ] Document parameter rationale

**Deliverable:** Final research report

---

## Expected Results

### Baseline Performance Targets

| Metric | Target | Stretch Goal |
|--------|--------|---------------|
| **CAGR** | 50-100% | >150% |
| **Sharpe Ratio** | 1.5-2.5 | >3.0 |
| **Max Drawdown** | <15% | <10% |
| **Calmar Ratio** | 3-5 | >7 |
| **Win Rate** | 50-60% | >65% |
| **Profit Factor** | 1.5-2.5 | >3.0 |

### Parameter Insights (Expected)

**High-Impact Parameters:**
1. **Position sizing (Kelly fraction)** - Expected impact: 20-30% on Sharpe
2. **Circuit breaker thresholds** - Expected impact: 10-15% on Max DD
3. **Signal fusion weights** - Expected impact: 15-25% on CAGR
4. **Volatility regime thresholds** - Expected impact: 10-20% on risk-adjusted returns

**Low-Impact Parameters:**
- Execution slicing (minor impact on costs)
- Exchange allocation (diversification benefit, minimal return impact)

---

## Monitoring Dashboard

### Real-Time Optimization Metrics

```python
class OptimizationDashboard:
    def __init__(self):
        self.best_score = 0
        self.iterations = []
        self.scores = []
        self.params_history = []
        
    def update(self, iteration, params, score):
        self.iterations.append(iteration)
        self.scores.append(score)
        self.params_history.append(params)
        
        if score > self.best_score:
            self.best_score = score
            self.best_params = params
        
        # Log progress
        logger.info(f"Iteration {iteration}: Score={score:.4f}, Best={self.best_score:.4f}")
        
        # Visualize
        if iteration % 50 == 0:
            self.plot_progress()
    
    def plot_progress(self):
        """Plot optimization progress."""
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        
        # Score over iterations
        axes[0, 0].plot(self.iterations, self.scores)
        axes[0, 0].axhline(self.best_score, color='r', linestyle='--', label='Best')
        axes[0, 0].set_xlabel('Iteration')
        axes[0, 0].set_ylabel('Score')
        axes[0, 0].set_title('Optimization Progress')
        
        # Parameter importance (top 10)
        importance = self.calculate_parameter_importance()
        top_10 = importance.nlargest(10)
        axes[0, 1].barh(top_10.index, top_10.values)
        axes[0, 1].set_xlabel('Importance')
        axes[0, 1].set_title('Top 10 Parameters')
        
        # Score distribution
        axes[1, 0].hist(self.scores, bins=50)
        axes[1, 0].set_xlabel('Score')
        axes[1, 0].set_title('Score Distribution')
        
        # Best params values
        best_params_flat = self.flatten_params(self.best_params)
        axes[1, 1].barh(list(best_params_flat.keys()), list(best_params_flat.values()))
        axes[1, 1].set_title('Best Parameter Values')
        
        plt.tight_layout()
        plt.savefig(f'optimization_progress_{len(self.iterations)}.png')
```

---

## Research Report Template

### Auto-Generated Report Sections

```markdown
# Crypto Algorithm Optimization Report

## Executive Summary
- Best CAGR: X%
- Best Sharpe: Y
- Best Max DD: Z%
- Total iterations: N

## Optimal Parameters

### Signal Generation
- On-chain weight: 0.XX
- Social weight: 0.XX
- Technical weight: 0.XX
- Funding weight: 0.XX

### Risk Management
- Kelly fraction: 0.XX
- Max position: X%
- Circuit breaker: X%

### Execution
- Num slices: X
- Execution horizon: X hours

## Performance Metrics

| Metric | Train | Validation | Out-of-Sample |
|--------|-------|------------|---------------|
| CAGR | X% | Y% | Z% |
| Sharpe | X | Y | Z |
| Max DD | X% | Y% | Z% |

## Parameter Importance
1. Kelly fraction (importance: 0.XX)
2. Social weight (importance: 0.XX)
3. Circuit breaker (importance: 0.XX)

## Robustness Analysis
- Out-of-sample degradation: X%
- Cross-exchange consistency: Y%
- Monte Carlo stability: Z%

## Recommendations
1. Use these parameters for live trading
2. Monitor parameter drift monthly
3. Re-optimize quarterly
```

---

## Computational Requirements

### Hardware

| Resource | Specification | Cost |
|----------|---------------|------|
| **GPU** | 10x NVIDIA A10G | $15/hour |
| **CPU** | 64 vCPUs | $3/hour |
| **RAM** | 512 GB | $5/hour |
| **Storage** | 5 TB SSD | $500/month |
| **Total (7 days)** | | ~$5,000 |

### Software

| Tool | Purpose | License |
|------|---------|---------|
| **Optuna** | Bayesian Optimization | MIT (free) |
| **Ray** | Distributed computing | Apache 2.0 (free) |
| **ccxt** | Crypto exchange API | MIT (free) |
| **web3.py** | On-chain data | MIT (free) |
| **PyTorch** | Deep learning | BSD (free) |

---

**Document Status:** Ready for implementation  
**Version:** 1.0  
**Timeline:** 4 weeks  
**Budget:** ~$10,000 (compute + data)  
**Expected ROI:** 50-150% CAGR improvement
