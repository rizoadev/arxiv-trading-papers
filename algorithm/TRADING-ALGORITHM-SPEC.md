# Trading Algorithm Specification
## AI-Powered Multi-Agent Trading System

**Version:** 1.0  
**Date:** March 16, 2026  
**Based on:** 142 arXiv papers (2024-2026)  
**Status:** Ready for Implementation

---

## Table of Contents

1. [Algorithm Overview](#algorithm-overview)
2. [Signal Generation](#signal-generation)
3. [Risk Management](#risk-management)
4. [Position Sizing](#position-sizing)
5. [Portfolio Construction](#portfolio-construction)
6. [Execution Logic](#execution-logic)
7. [Performance Metrics](#performance-metrics)
8. [Pseudocode](#pseudocode)

---

## Algorithm Overview

### Core Philosophy

**Hybrid Approach:** Combine LLM reasoning with traditional quantitative models

```
Trading Decision = f(LLM_Analysis, Quant_Signals, Risk_Constraints)
```

**Key Principles:**
1. **Multi-Agent Consensus**: No single agent makes decisions alone
2. **Risk-First**: Risk management overrides all signals
3. **Adaptive**: Adjust to market regimes (calm, elevated, crisis)
4. **Cost-Aware**: Minimize slippage and transaction costs
5. **Explainable**: Every decision has audit trail

---

### High-Level Flow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. DATA INGESTION (Real-time)                               │
│    - Market data (OHLCV, order book)                        │
│    - Alternative data (news, social media)                  │
│    - Fundamentals (earnings, SEC filings)                   │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 2. SIGNAL GENERATION (Parallel Agents)                      │
│    ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │
│    │ Fundamental │  │  Sentiment  │  │  Technical  │       │
│    │   (LLM)     │  │   (LLM)     │  │  (LSTM-GNN) │       │
│    │  Bullish    │  │  Neutral    │  │   Bearish   │       │
│    │  +0.6       │  │   0.0       │  │   -0.4      │       │
│    └─────────────┘  └─────────────┘  └─────────────┘       │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 3. SIGNAL FUSION (Manager Agent)                            │
│    - Weighted average: (0.4×0.6) + (0.3×0.0) + (0.3×-0.4)   │
│    - Raw signal: +0.12 (weak bullish)                       │
│    - Confidence: 0.65 (moderate)                            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 4. RISK OVERLAY (GARCH-GRU)                                 │
│    - Current volatility: 0.18 (elevated)                    │
│    - VaR 95%: -3.2%                                         │
│    - Risk regime: "elevated" → reduce exposure 50%          │
│    - Adjusted signal: +0.12 × 0.5 = +0.06                   │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 5. POSITION SIZING (Kelly + Constraints)                    │
│    - Kelly fraction: 0.06 / 0.18² = 0.19 (19%)              │
│    - Apply constraints: max 10% per position                │
│    - Final size: 10% of portfolio                           │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 6. EXECUTION (VWAP Transformer)                             │
│    - Slice order: 10% → 5 slices over 4 hours               │
│    - Expected slippage: 3.5 bps                             │
│    - Send to broker                                         │
└─────────────────────────────────────────────────────────────┘
```

---

## Signal Generation

### 1. Fundamental Analyst (LLM)

**Input:** SEC filings, earnings calls, analyst reports  
**Output:** Sentiment score [-1, +1] with confidence

```python
@dataclass
class FundamentalSignal:
    symbol: str
    sentiment: float  # -1 to +1
    confidence: float  # 0 to 1
    factors: List[str]
    citations: List[str]
    timestamp: datetime

def analyze_fundamentals(symbol: str, documents: List[Document]) -> FundamentalSignal:
    """
    Analyze fundamental data using LLM.
    Based on: TradingAgents (2412.20138), Trading-R1 (2509.11420)
    """
    prompt = f"""
    Analyze {symbol} based on these documents.
    
    Output JSON:
    {{
        "sentiment": -1.0 to +1.0,
        "confidence": 0.0 to 1.0,
        "factors": ["positive/negative factors"],
        "citations": ["document references"]
    }}
    
    Documents:
    {documents}
    """
    
    response = llm_call(prompt, model="gpt-4-turbo")
    return parse_fundamental_signal(response)
```

**Scoring Rules:**

| Factor | Score Impact |
|--------|--------------|
| Revenue beat >10% | +0.3 |
| EPS beat >10% | +0.2 |
| Guidance raise | +0.2 |
| Margin expansion | +0.1 |
| Revenue miss >10% | -0.3 |
| EPS miss >10% | -0.2 |
| Guidance cut | -0.2 |
| Margin contraction | -0.1 |

**Final Score**: Sum of factors, capped at [-1, +1]

---

### 2. Sentiment Analyst (DPO-LLM)

**Input:** News, social media, Reddit, Twitter  
**Output:** Sentiment score [-1, +1] with momentum

```python
@dataclass
class SentimentSignal:
    symbol: str
    sentiment: float  # -1 to +1
    momentum: str  # increasing, decreasing, stable
    volume: int  # number of mentions
    anomaly: bool  # unusual activity detected
    timestamp: datetime

def analyze_sentiment(symbol: str, texts: List[str]) -> SentimentSignal:
    """
    Analyze sentiment from news and social media.
    Based on: FinDPO (2507.18417), Attention Factors (2510.11616)
    """
    # Aggregate recent texts (last 24h)
    combined = " ".join(texts[-100:])
    
    # DPO-tuned model for finance sentiment
    prompt = f"""
    Analyze sentiment for {symbol}:
    {combined[:2000]}
    
    Output: sentiment_score (-1 to +1), momentum, volume, anomaly
    """
    
    response = llm_call(prompt, model="claude-3-haiku")
    return parse_sentiment_signal(response)
```

**Momentum Calculation:**

```python
def calculate_momentum(recent_sentiment: float, older_sentiment: float) -> str:
    if recent_sentiment > older_sentiment * 1.5:
        return "increasing"
    elif recent_sentiment < older_sentiment * 0.5:
        return "decreasing"
    else:
        return "stable"
```

---

### 3. Technical Analyst (LSTM-GNN)

**Input:** OHLCV, technical indicators, inter-stock correlations  
**Output:** Price prediction with probability

```python
@dataclass
class TechnicalSignal:
    symbol: str
    direction: str  # up, down, neutral
    probability: float  # 0 to 1
    expected_move: float  # percentage
    confidence_interval: tuple
    technical_levels: dict
    timestamp: datetime

def analyze_technicals(symbol: str, 
                       price_data: pd.DataFrame,
                       correlation_matrix: pd.DataFrame) -> TechnicalSignal:
    """
    Predict price direction using LSTM-GNN hybrid.
    Based on: LSTM-GNN (2502.15813) - 10.6% MSE improvement
    """
    # Prepare features
    features = prepare_features(price_data)  # 20 features
    adj_matrix = prepare_adjacency(correlation_matrix)
    
    # Load model
    model = LSTMGNNTechnicalAnalyst.load("models/lstm_gnn.pth")
    
    # Predict
    with torch.no_grad():
        pred = model(features, adj_matrix)
    
    # Convert to signal
    direction = "up" if pred > 0 else "down"
    probability = 1 / (1 + np.exp(-pred * 10))  # Sigmoid
    expected_move = abs(pred) * 100
    
    return TechnicalSignal(
        symbol=symbol,
        direction=direction,
        probability=probability,
        expected_move=expected_move,
        confidence_interval=calculate_ci(pred),
        technical_levels=get_technical_levels(price_data)
    )
```

**Technical Indicators Used:**

| Indicator | Parameters | Purpose |
|-----------|------------|---------|
| RSI | 14-period | Overbought/oversold |
| MACD | 12,26,9 | Trend momentum |
| Bollinger Bands | 20,2 | Volatility bands |
| ATR | 14-period | Volatility measure |
| ADX | 14-period | Trend strength |
| OBV | Cumulative | Volume confirmation |

---

### 4. Signal Fusion (Manager Agent)

**Input:** Signals from 3 agents  
**Output:** Combined signal with confidence

```python
@dataclass
class CombinedSignal:
    symbol: str
    raw_signal: float  # -1 to +1
    confidence: float  # 0 to 1
    agent_signals: dict
    rationale: str
    timestamp: datetime

def fuse_signals(fundamental: FundamentalSignal,
                 sentiment: SentimentSignal,
                 technical: TechnicalSignal) -> CombinedSignal:
    """
    Combine signals from all agents using adaptive weighting.
    Based on: ATLAS (2510.15949), TradingAgents (2412.20138)
    """
    # Dynamic weights based on confidence
    weights = {
        'fundamental': fundamental.confidence * 0.4,
        'sentiment': 0.3,  # Fixed weight
        'technical': technical.probability * 0.3
    }
    
    # Normalize weights
    total_weight = sum(weights.values())
    weights = {k: v/total_weight for k, v in weights.items()}
    
    # Calculate raw signal
    raw_signal = (
        weights['fundamental'] * fundamental.sentiment +
        weights['sentiment'] * sentiment.sentiment +
        weights['technical'] * (1 if technical.direction == 'up' else -1) * technical.probability
    )
    
    # Calculate confidence (weighted average)
    confidence = (
        weights['fundamental'] * fundamental.confidence +
        weights['sentiment'] * 0.7 +  # Assume fixed confidence for sentiment
        weights['technical'] * technical.probability
    )
    
    # Generate rationale using LLM
    rationale = generate_rationale(fundamental, sentiment, technical, weights)
    
    return CombinedSignal(
        symbol=fundamental.symbol,
        raw_signal=raw_signal,
        confidence=confidence,
        agent_signals={
            'fundamental': fundamental.sentiment,
            'sentiment': sentiment.sentiment,
            'technical': technical.direction
        },
        rationale=rationale
    )
```

**Weight Adaptation by Market Regime:**

| Regime | Fundamental | Sentiment | Technical |
|--------|-------------|-----------|-----------|
| **Calm** | 40% | 20% | 40% |
| **Elevated** | 50% | 20% | 30% |
| **Crisis** | 60% | 10% | 30% |

---

## Risk Management

### 1. Volatility Forecasting (GARCH-GRU)

**Input:** Return series  
**Output:** Volatility forecast, VaR, CVaR

```python
@dataclass
class RiskMetrics:
    current_volatility: float
    forecast_1d: float
    forecast_5d: float
    var_95: float
    cvar_95: float
    risk_regime: str  # calm, normal, elevated, crisis
    position_limit: float  # max position size
    timestamp: datetime

def assess_risk(returns: np.ndarray) -> RiskMetrics:
    """
    Forecast volatility and calculate risk metrics.
    Based on: GARCH-GRU (2504.09380)
    """
    # Load model
    model = GARCHGRU.load("models/garch_gru.pth")
    
    # Forecast volatility
    with torch.no_grad():
        vol_forecast = model(torch.FloatTensor(returns))
    
    current_vol = vol_forecast[0, -1].item()
    forecast_1d = current_vol
    forecast_5d = current_vol * np.sqrt(5)
    
    # Calculate VaR and CVaR
    var_95 = current_vol * norm.ppf(0.05)
    cvar_95 = current_vol * (-norm.pdf(norm.ppf(0.05)) / 0.05)
    
    # Determine regime
    regime = determine_regime(current_vol)
    
    # Set position limits
    position_limit = get_position_limit(regime)
    
    return RiskMetrics(
        current_volatility=current_vol,
        forecast_1d=forecast_1d,
        forecast_5d=forecast_5d,
        var_95=var_95,
        cvar_95=cvar_95,
        risk_regime=regime,
        position_limit=position_limit
    )
```

**Regime Detection:**

```python
def determine_regime(volatility: float, 
                     historical_mean: float,
                     historical_std: float) -> str:
    if volatility > historical_mean + 3 * historical_std:
        return "crisis"
    elif volatility > historical_mean + 2 * historical_std:
        return "elevated"
    elif volatility > historical_mean - historical_std:
        return "normal"
    else:
        return "calm"
```

**Position Limits by Regime:**

| Regime | Max Position | Max Portfolio Exposure | Max Leverage |
|--------|--------------|------------------------|--------------|
| **Calm** | 15% | 100% | 1.5x |
| **Normal** | 10% | 80% | 1.2x |
| **Elevated** | 5% | 50% | 1.0x |
| **Crisis** | 0% (close all) | 20% | 0.5x |

---

### 2. Circuit Breakers

**Purpose:** Stop trading during extreme conditions

```python
class CircuitBreaker:
    def __init__(self):
        self.daily_loss_limit = 0.05  # 5%
        self.max_drawdown = 0.10  # 10%
        self.var_breach_limit = 3  # 3 VaR breaches/day
        self.consecutive_losses = 5
        
    def check(self, portfolio: Portfolio, today_pnl: float) -> bool:
        """
        Return True if trading should stop.
        Based on: TradeTrap (2512.02261)
        """
        # Daily loss limit
        if today_pnl < -self.daily_loss_limit:
            logger.warning("Daily loss limit breached")
            return True
        
        # Max drawdown
        if portfolio.drawdown > self.max_drawdown:
            logger.warning("Max drawdown breached")
            return True
        
        # VaR breaches
        var_breaches = count_var_breaches(today_pnl, portfolio.var_95)
        if var_breaches > self.var_breach_limit:
            logger.warning("Too many VaR breaches")
            return True
        
        # Consecutive losses
        if portfolio.consecutive_losses >= self.consecutive_losses:
            logger.warning("Consecutive losses limit")
            return True
        
        return False
```

---

## Position Sizing

### Kelly Criterion with Constraints

```python
def calculate_position_size(signal: CombinedSignal,
                            risk_metrics: RiskMetrics,
                            portfolio_value: float) -> float:
    """
    Calculate optimal position size using Kelly criterion with constraints.
    Based on: Attention Factors (2510.11616), FinPos (2510.27251)
    """
    # Kelly fraction: f* = (p * b - q) / b
    # Where: p = win probability, q = loss probability, b = win/loss ratio
    
    p = signal.confidence  # Win probability
    q = 1 - p  # Loss probability
    b = abs(signal.raw_signal) / risk_metrics.current_volatility  # Win/loss ratio
    
    # Kelly fraction
    kelly_fraction = (p * b - q) / b if b > 0 else 0
    
    # Apply constraints
    kelly_fraction = min(kelly_fraction, risk_metrics.position_limit)
    kelly_fraction = max(kelly_fraction, 0)  # No shorting for now
    
    # Half-Kelly (more conservative)
    kelly_fraction = kelly_fraction * 0.5
    
    # Calculate position value
    position_value = portfolio_value * kelly_fraction
    
    return position_value
```

**Position Sizing Example:**

| Input | Value |
|-------|-------|
| Signal | +0.12 (weak bullish) |
| Confidence | 0.65 |
| Volatility | 0.18 |
| Kelly Fraction | (0.65 × 0.67 - 0.35) / 0.67 = 0.13 |
| Position Limit (elevated) | 0.05 |
| Final Kelly | min(0.13, 0.05) × 0.5 = 0.025 |
| Portfolio Value | $1,000,000 |
| **Position Size** | **$25,000 (2.5%)** |

---

## Portfolio Construction

### Mean-Variance Optimization with RL

```python
def optimize_portfolio(signals: List[CombinedSignal],
                       risk_metrics: RiskMetrics,
                       constraints: dict) -> Portfolio:
    """
    Construct optimal portfolio using mean-variance with RL adjustments.
    Based on: KDD (2405.05449), MTS (2503.04143)
    """
    # Extract expected returns from signals
    expected_returns = {s.symbol: s.raw_signal for s in signals}
    
    # Build covariance matrix (from GARCH-GRU forecasts)
    cov_matrix = build_covariance_matrix(signals, risk_metrics)
    
    # Mean-variance optimization
    weights = cvxpy.Variable(len(signals))
    risk = cvxpy.quad_form(weights, cov_matrix)
    return_param = expected_returns @ weights
    
    # Objective: Maximize Sharpe ratio
    objective = cvxpy.Maximize(return_param / cvxpy.sqrt(risk))
    
    # Constraints
    constraints_list = [
        cvxpy.sum(weights) == 1,
        weights >= 0,  # No shorting
        weights <= constraints['max_position'],
    ]
    
    # Solve
    problem = cvxpy.Problem(objective, constraints_list)
    problem.solve()
    
    # RL adjustment (learned from past performance)
    rl_adjustment = get_rl_adjustment(weights.value)
    final_weights = weights.value * rl_adjustment
    
    # Normalize
    final_weights = final_weights / final_weights.sum()
    
    return Portfolio(weights=final_weights)
```

**Diversification Rules:**

| Rule | Limit |
|------|-------|
| Max single position | 10% |
| Max sector exposure | 30% |
| Max correlation between positions | 0.7 |
| Min number of positions | 5 |
| Max number of positions | 20 |

---

## Execution Logic

### VWAP Transformer Execution

```python
@dataclass
class ExecutionPlan:
    total_quantity: int
    slices: List[OrderSlice]
    expected_slippage_bps: float
    expected_completion_time: datetime

@dataclass
class OrderSlice:
    time: datetime
    quantity: int
    order_type: str  # LIMIT, MARKET
    price: Optional[float]  # For limit orders

def execute_order(symbol: str,
                  quantity: int,
                  side: str,
                  current_price: float) -> ExecutionPlan:
    """
    Execute order using VWAP Transformer for optimal slicing.
    Based on: VWAP Signature Transformer (2503.02680)
    """
    # Load model
    model = VWAPTransformer.load("models/vwap_transformer.pth")
    
    # Prepare features
    features = prepare_execution_features(
        symbol=symbol,
        quantity=quantity,
        side=side,
        current_price=current_price,
        historical_volume=get_volume_profile(symbol),
        order_book=get_order_book(symbol)
    )
    
    # Predict optimal execution strategy
    with torch.no_grad():
        strategy = model(features)
    
    # Generate slices
    slices = []
    remaining = quantity
    
    for slice_pct, time_offset in strategy:
        slice_qty = int(quantity * slice_pct)
        slice_qty = min(slice_qty, remaining)
        
        slice = OrderSlice(
            time=datetime.now() + timedelta(minutes=time_offset),
            quantity=slice_qty,
            order_type="LIMIT",
            price=current_price * (1 - 0.001)  # 10 bps discount for buy
        )
        slices.append(slice)
        remaining -= slice_qty
    
    # Expected slippage
    expected_slippage = strategy.expected_slippage_bps
    
    return ExecutionPlan(
        total_quantity=quantity,
        slices=slices,
        expected_slippage_bps=expected_slippage,
        expected_completion_time=slices[-1].time
    )
```

**Execution Parameters:**

| Parameter | Value |
|-----------|-------|
| Number of slices | 5-10 |
| Execution horizon | 2-6 hours |
| Limit order discount | 5-15 bps |
| Max market order % | 20% |
| Expected slippage | 3-5 bps |

---

## Performance Metrics

### Real-time Monitoring

```python
@dataclass
class PerformanceMetrics:
    # Returns
    daily_return: float
    weekly_return: float
    monthly_return: float
    ytd_return: float
    total_return: float
    
    # Risk-adjusted
    sharpe_ratio: float
    sortino_ratio: float
    calmar_ratio: float
    
    # Risk
    max_drawdown: float
    current_drawdown: float
    var_95: float
    beta: float
    
    # Trading
    win_rate: float
    profit_factor: float
    avg_win: float
    avg_loss: float
    total_trades: int
    
    # Costs
    total_slippage_bps: float
    total_commission: float
    turnover_rate: float
```

**Calculation Methods:**

```python
def calculate_sharpe(returns: np.ndarray, risk_free_rate: float = 0.02) -> float:
    """Calculate Sharpe ratio."""
    excess_returns = returns - risk_free_rate / 252
    return np.sqrt(252) * excess_returns.mean() / excess_returns.std()

def calculate_sortino(returns: np.ndarray, risk_free_rate: float = 0.02) -> float:
    """Calculate Sortino ratio (downside deviation)."""
    excess_returns = returns - risk_free_rate / 252
    downside_returns = returns[returns < 0]
    downside_std = np.sqrt((downside_returns ** 2).mean())
    return np.sqrt(252) * excess_returns.mean() / downside_std if downside_std > 0 else 0

def calculate_max_drawdown(equity_curve: np.ndarray) -> float:
    """Calculate maximum drawdown."""
    running_max = np.maximum.accumulate(equity_curve)
    drawdown = (equity_curve - running_max) / running_max
    return abs(drawdown.min())

def calculate_profit_factor(gross_profit: float, gross_loss: float) -> float:
    """Calculate profit factor."""
    return gross_profit / abs(gross_loss) if gross_loss != 0 else float('inf')
```

---

## Pseudocode

### Main Trading Loop

```python
def main_trading_loop():
    """
    Main trading loop - runs every minute during market hours.
    """
    # Initialize
    portfolio = Portfolio(initial_capital=1_000_000)
    circuit_breaker = CircuitBreaker()
    
    while market_is_open():
        # 1. Get current positions and P&L
        today_pnl = portfolio.calculate_daily_pnl()
        
        # 2. Check circuit breakers
        if circuit_breaker.check(portfolio, today_pnl):
            logger.warning("Circuit breaker triggered - stopping trading")
            close_all_positions()
            break
        
        # 3. Get universe of tradable symbols
        universe = get_trading_universe()
        
        # 4. Generate signals for each symbol
        signals = []
        for symbol in universe:
            # Parallel agent execution
            fundamental = analyze_fundamentals(symbol)
            sentiment = analyze_sentiment(symbol)
            technical = analyze_technicals(symbol)
            
            # Fuse signals
            combined = fuse_signals(fundamental, sentiment, technical)
            signals.append(combined)
        
        # 5. Get risk metrics
        returns = get_historical_returns(universe)
        risk_metrics = assess_risk(returns)
        
        # 6. Filter signals by confidence and risk
        tradable_signals = [
            s for s in signals 
            if s.confidence > 0.6 and abs(s.raw_signal) > 0.1
        ]
        
        # 7. Optimize portfolio
        if tradable_signals:
            target_portfolio = optimize_portfolio(
                tradable_signals, 
                risk_metrics,
                constraints={
                    'max_position': risk_metrics.position_limit,
                    'max_sector': 0.30,
                    'max_correlation': 0.70
                }
            )
            
            # 8. Generate trades (rebalance)
            trades = generate_trades(portfolio, target_portfolio)
            
            # 9. Execute trades
            for trade in trades:
                execution_plan = execute_order(
                    symbol=trade.symbol,
                    quantity=trade.quantity,
                    side=trade.side,
                    current_price=get_current_price(trade.symbol)
                )
                
                # Send to broker
                for slice in execution_plan.slices:
                    send_order_to_broker(slice)
        
        # 10. Update risk metrics
        portfolio.update_var(risk_metrics.var_95)
        
        # 11. Log metrics
        log_performance_metrics(portfolio)
        
        # Wait for next minute
        sleep(60)
```

### Signal Generation (Detailed)

```python
def generate_signals(universe: List[str]) -> List[CombinedSignal]:
    """
    Generate trading signals for all symbols in universe.
    Runs in parallel for efficiency.
    """
    signals = []
    
    # Parallel execution
    with ThreadPoolExecutor(max_workers=10) as executor:
        futures = {
            executor.submit(process_symbol, symbol): symbol 
            for symbol in universe
        }
        
        for future in as_completed(futures):
            symbol = futures[future]
            try:
                signal = future.result()
                signals.append(signal)
            except Exception as e:
                logger.error(f"Error processing {symbol}: {e}")
    
    return signals

def process_symbol(symbol: str) -> CombinedSignal:
    """
    Process single symbol: get all agent signals and fuse.
    """
    # Get data
    documents = get_fundamental_documents(symbol)
    news = get_news_articles(symbol)
    price_data = get_price_data(symbol)
    correlation_matrix = get_correlation_matrix()
    
    # Run agents
    fundamental = analyze_fundamentals(symbol, documents)
    sentiment = analyze_sentiment(symbol, news)
    technical = analyze_technicals(symbol, price_data, correlation_matrix)
    
    # Fuse
    combined = fuse_signals(fundamental, sentiment, technical)
    
    return combined
```

---

## Summary

### Algorithm Characteristics

| Characteristic | Value |
|----------------|-------|
| **Decision Frequency** | Every minute |
| **Holding Period** | 1-5 days (average) |
| **Max Positions** | 20 |
| **Max Position Size** | 10% |
| **Max Portfolio Exposure** | 80% (normal regime) |
| **Target Sharpe** | 1.5-2.0 |
| **Max Drawdown** | 10% (circuit breaker) |
| **Expected Turnover** | 50-100%/month |
| **Expected Slippage** | 3-5 bps per trade |

### Key Innovations from Papers

1. **Multi-Agent Consensus** (TradingAgents, ATLAS)
2. **LLM + RL Fusion** (Trading-R1, FLAG-Trader)
3. **GARCH-GRU Volatility** (2504.09380)
4. **LSTM-GNN Technical** (2502.15813)
5. **VWAP Transformer Execution** (2503.02680)
6. **Kelly + Risk Constraints** (Attention Factors)
7. **Circuit Breakers** (TradeTrap)
8. **Adaptive-OPRO** (ATLAS)

---

**Document Status:** Ready for implementation  
**Version:** 1.0  
**Next Step:** Code implementation
