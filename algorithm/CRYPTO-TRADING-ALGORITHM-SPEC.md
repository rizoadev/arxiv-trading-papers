# Crypto Trading Algorithm Specification
## AI-Powered Multi-Agent Crypto Trading System

**Version:** 1.0  
**Date:** March 16, 2026  
**Based on:** 142 arXiv papers + Crypto-specific research  
**Markets:** Bitcoin, Ethereum, Top 50 Altcoins  
**Status:** Ready for Implementation

---

## Executive Summary

### Crypto vs Traditional Markets

| Characteristic | Traditional | Crypto | Adaptation |
|----------------|-------------|--------|------------|
| **Trading Hours** | 9:30-16:00 EST | 24/7/365 | Continuous monitoring |
| **Volatility** | 15-25% annual | 60-100% annual | Smaller position sizes |
| **Liquidity** | High (large caps) | Variable (exchange-dependent) | Multi-exchange execution |
| **Settlement** | T+2 | Instant | Faster rebalancing |
| **Shorting** | Regulated | Easy (perpetual swaps) | Long/short symmetric |
| **Leverage** | 2-4x max | Up to 100x | Cap at 5x for safety |
| **Market Structure** | Centralized | CEX + DEX | Arbitrage opportunities |
| **Data Sources** | SEC filings, earnings | On-chain, social, dev activity | Different alpha sources |
| **Regulation** | High | Low/Evolving | Higher risk tolerance |

### Key Crypto Adaptations

1. **24/7 Operation**: 3-shift monitoring system
2. **Higher Volatility**: Position sizes 50% of traditional
3. **On-Chain Data**: Add blockchain metrics to signals
4. **Social Sentiment**: 3x weight vs traditional (crypto more sentiment-driven)
5. **Multi-Exchange**: Execute across Binance, Coinbase, Kraken, DEXs
6. **Perpetual Swaps**: Use perps for long/short (no expiry)
7. **Funding Rates**: Incorporate perp funding into signals
8. **Stablecoin Yield**: Idle capital earns 5-10% APY

---

## Modified Algorithm Architecture

### Crypto-Specific Flow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. DATA INGESTION (24/7)                                    │
│    - Price/volume (10+ exchanges)                           │
│    - On-chain metrics (Glassnode, Dune)                     │
│    - Social sentiment (Twitter, Reddit, Telegram)           │
│    - Funding rates (perpetual swaps)                        │
│    - Developer activity (GitHub)                            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ 2. SIGNAL GENERATION (Crypto Agents)                        │
│    ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │
│    │  On-Chain   │  │   Social    │  │  Technical  │       │
│    │   Analyst   │  │  Sentiment  │  │  (LSTM-GNN) │       │
│    │  (LLM)      │  │  (LLM)      │  │             │       │
│    │  +0.4       │  │  +0.8       │  │   +0.3      │       │
│    └─────────────┘  └─────────────┘  └─────────────┘       │
│         │                  │                  │             │
│         └──────────────────┼──────────────────┘             │
│                            │                                │
│              ┌─────────────▼─────────────┐                 │
│              │    Funding Rate Signal    │                 │
│              │      (Carry Trade)        │                 │
│              │       +0.1 (long)         │                 │
│              └─────────────┬─────────────┘                 │
└────────────────────────────┼───────────────────────────────┘
                             │
┌────────────────────────────▼───────────────────────────────┐
│ 3. CRYPTO SIGNAL FUSION (Manager)                          │
│    - Weights: On-chain 25%, Social 35%, Technical 25%,     │
│               Funding 15%                                  │
│    - Raw signal: +0.47 (moderate bullish)                  │
│    - Confidence: 0.72                                      │
└────────────────────────────┬───────────────────────────────┘
                             │
┌────────────────────────────▼───────────────────────────────┐
│ 4. CRYPTO RISK OVERLAY                                     │
│    - Volatility: 0.65 (crypto normal)                      │
│    - Correlation to BTC: 0.85                              │
│    - Exchange risk: Diversify across 3+                    │
│    - Position limit: 5% (vs 10% traditional)               │
│    - Leverage: 3x (capped at 5x max)                       │
└────────────────────────────┬───────────────────────────────┘
                             │
┌────────────────────────────▼───────────────────────────────┐
│ 5. MULTI-EXCHANGE EXECUTION                                │
│    - Split across Binance (40%), Coinbase (30%),           │
│      Kraken (20%), DEX (10%)                              │
│    - Minimize slippage + optimize fees                     │
│    - Monitor for arbitrage opportunities                   │
└────────────────────────────────────────────────────────────┘
```

---

## Crypto-Specific Signal Generation

### 1. On-Chain Analyst (LLM)

**Input:** Blockchain metrics, whale movements, exchange flows  
**Output:** On-chain sentiment score

```python
@dataclass
class OnChainSignal:
    symbol: str  # e.g., "BTC", "ETH"
    sentiment: float  # -1 to +1
    confidence: float
    metrics: dict
    whale_activity: str
    exchange_flow: str
    timestamp: datetime

def analyze_onchain(symbol: str) -> OnChainSignal:
    """
    Analyze on-chain metrics using LLM.
    Based on: CryptoTrade (2407.09546), WebCryptoAgent (2601.04687)
    """
    # Get on-chain data
    metrics = {
        'active_addresses': get_active_addresses(symbol, days=30),
        'transaction_volume': get_transaction_volume(symbol, days=30),
        'exchange_inflow': get_exchange_inflow(symbol, days=7),
        'exchange_outflow': get_exchange_outflow(symbol, days=7),
        'whale_transactions': get_whale_transactions(symbol, threshold=1000000),
        'hash_rate': get_hash_rate(symbol) if symbol == 'BTC' else None,
        'staking_ratio': get_staking_ratio(symbol) if symbol in ['ETH', 'SOL'] else None,
        'futures_oi': get_futures_open_interest(symbol),
    }
    
    # LLM analysis
    prompt = f"""
    Analyze on-chain metrics for {symbol}:
    
    {json.dumps(metrics, indent=2)}
    
    Output JSON:
    {{
        "sentiment": -1.0 to +1.0,
        "confidence": 0.0 to 1.0,
        "whale_activity": "accumulating|distributing|neutral",
        "exchange_flow": "inflow|outflow|neutral",
        "key_observations": ["observation 1", ...]
    }}
    
    Interpretation guide:
    - Exchange outflow + whale accumulation = bullish
    - Exchange inflow + whale distribution = bearish
    - Rising active addresses = bullish adoption
    - Rising hash rate (BTC) = bullish security
    """
    
    response = llm_call(prompt, model="claude-3-sonnet")
    return parse_onchain_signal(response)
```

**On-Chain Scoring:**

| Metric | Bullish Signal | Score Impact |
|--------|----------------|--------------|
| Exchange Outflow | >10K BTC/day | +0.3 |
| Exchange Inflow | >10K BTC/day | -0.3 |
| Whale Accumulation | >100 addresses | +0.2 |
| Whale Distribution | >100 addresses | -0.2 |
| Active Addresses | +20% MoM | +0.2 |
| Active Addresses | -20% MoM | -0.2 |
| Hash Rate (BTC) | ATH | +0.1 |
| Futures OI | Rising + price up | +0.1 |
| Futures OI | Rising + price down | -0.1 |

---

### 2. Social Sentiment (Enhanced for Crypto)

**Input:** Twitter, Reddit, Telegram, Discord, YouTube  
**Output:** Social sentiment with viral detection

```python
@dataclass
class SocialSentiment:
    symbol: str
    sentiment: float  # -1 to +1
    volume: int  # mentions/24h
    momentum: str  # viral, trending, stable, dying
    influencer_sentiment: float  # weighted by follower count
    retail_sentiment: float
    fud_score: float  # fear, uncertainty, doubt
    timestamp: datetime

def analyze_crypto_sentiment(symbol: str) -> SocialSentiment:
    """
    Analyze crypto social sentiment.
    Based on: CryptoTrade (2407.09546), FinDPO (2507.18417)
    """
    # Collect from multiple sources
    sources = {
        'twitter': get_twitter_mentions(symbol, hours=24),
        'reddit': get_reddit_posts(['cryptocurrency', 'bitcoin', 'ethtrader'], symbol),
        'telegram': get_telegram_signals(symbol),  # If available
        'youtube': get_youtube_videos(symbol, days=3),
        'news': get_crypto_news(symbol, hours=24),
    }
    
    # Separate influencer vs retail
    influencer_posts = [p for p in sources['twitter'] if p.followers > 100000]
    retail_posts = [p for p in sources['twitter'] if p.followers <= 100000]
    
    # Analyze with LLM
    influencer_sentiment = analyze_batch(influencer_posts, model="claude-3-haiku")
    retail_sentiment = analyze_batch(retail_posts, model="claude-3-haiku")
    
    # Detect viral momentum
    volume_24h = sum(len(s) for s in sources.values())
    volume_7d_avg = get_7day_average_volume(symbol)
    momentum_ratio = volume_24h / volume_7d_avg
    
    if momentum_ratio > 5:
        momentum = "viral"
    elif momentum_ratio > 2:
        momentum = "trending"
    elif momentum_ratio > 0.8:
        momentum = "stable"
    else:
        momentum = "dying"
    
    # FUD detection
    fud_score = detect_fud(sources)
    
    # Weighted sentiment (influencers 60%, retail 40%)
    overall_sentiment = 0.6 * influencer_sentiment + 0.4 * retail_sentiment
    
    return SocialSentiment(
        symbol=symbol,
        sentiment=overall_sentiment,
        volume=volume_24h,
        momentum=momentum,
        influencer_sentiment=influencer_sentiment,
        retail_sentiment=retail_sentiment,
        fud_score=fud_score
    )
```

**Crypto Social Weight:**

| Platform | Weight | Reason |
|----------|--------|--------|
| **Twitter (Influencers)** | 35% | Market movers |
| **Twitter (Retail)** | 15% | Crowd sentiment |
| **Reddit** | 20% | Deep discussion |
| **Telegram/Discord** | 15% | Early signals |
| **YouTube** | 10% | Longer-form analysis |
| **News** | 5% | Formal announcements |

---

### 3. Funding Rate Signal (Crypto-Specific)

**Input:** Perpetual swap funding rates across exchanges  
**Output:** Carry trade signal

```python
@dataclass
class FundingSignal:
    symbol: str
    avg_funding_rate: float  # annualized %
    funding_trend: str  # rising, falling, stable
    exchange_spread: float  # max - min across exchanges
    signal: str  # long, short, neutral
    expected_carry: float  # annualized return
    timestamp: datetime

def analyze_funding_rates(symbol: str) -> FundingSignal:
    """
    Analyze perpetual swap funding rates for carry trade.
    Based on: Crypto arbitrage strategies
    """
    exchanges = ['binance', 'bybit', 'okx', 'kraken', 'coinbase']
    funding_rates = {}
    
    for exchange in exchanges:
        rate = get_funding_rate(symbol, exchange)
        funding_rates[exchange] = rate
    
    # Calculate metrics
    avg_rate = np.mean(list(funding_rates.values()))
    max_rate = max(funding_rates.values())
    min_rate = min(funding_rates.values())
    spread = max_rate - min_rate
    
    # Annualize (funding is typically 8-hour)
    annualized_rate = avg_rate * 3 * 365
    
    # Determine signal
    if annualized_rate > 0.20:  # >20% APY
        signal = "short"  # Short perp, collect funding
        expected_carry = annualized_rate
    elif annualized_rate < -0.20:
        signal = "long"  # Long perp, collect funding
        expected_carry = abs(annualized_rate)
    else:
        signal = "neutral"
        expected_carry = 0
    
    # Trend
    historical = get_historical_funding(symbol, days=7)
    if avg_rate > np.mean(historical):
        trend = "rising"
    elif avg_rate < np.mean(historical):
        trend = "falling"
    else:
        trend = "stable"
    
    return FundingSignal(
        symbol=symbol,
        avg_funding_rate=annualized_rate,
        funding_trend=trend,
        exchange_spread=spread,
        signal=signal,
        expected_carry=expected_carry
    )
```

**Funding Rate Strategy:**

| Funding Rate (Annualized) | Signal | Action |
|---------------------------|--------|--------|
| >50% | Strong Short | Short perp, collect 50%+ APY |
| 20-50% | Short | Short perp, collect funding |
| -20% to +20% | Neutral | No carry trade |
| -50% to -20% | Long | Long perp, collect funding |
| <-50% | Strong Long | Long perp, collect 50%+ APY |

**Risk:** Funding can turn negative quickly. Monitor daily.

---

### 4. Technical Analyst (Crypto-Adapted LSTM-GNN)

**Modifications for Crypto:**

```python
def analyze_crypto_technicals(symbol: str,
                              price_data: pd.DataFrame,
                              btc_correlation: float) -> TechnicalSignal:
    """
    Crypto-adapted technical analysis.
    Based on: LSTM-GNN (2502.15813), CryptoTrade (2407.09546)
    """
    # Crypto-specific features
    crypto_features = {
        'btc_correlation': btc_correlation,  # Crypto moves with BTC
        'eth_correlation': get_correlation(symbol, 'ETH'),
        'futures_basis': get_futures_basis(symbol),
        'funding_rate': get_funding_rate(symbol, 'binance'),
        'open_interest': get_open_interest(symbol),
        'liquidation_volume': get_liquidation_volume(symbol, hours=24),
        'order_book_imbalance': get_order_book_imbalance(symbol),
    }
    
    # Traditional features (same as stocks)
    traditional_features = prepare_features(price_data)
    
    # Combine
    all_features = {**traditional_features, **crypto_features}
    
    # Load crypto-trained model
    model = LSTMGNNTechnicalAnalyst.load("models/lstm_gnn_crypto.pth")
    
    # Predict
    prediction = model(all_features)
    
    # Crypto adjustment: higher volatility → wider confidence intervals
    crypto_volatility = get_crypto_volatility(symbol)
    confidence_adjustment = 0.8  # Reduce confidence vs stocks
    
    return TechnicalSignal(
        symbol=symbol,
        direction="up" if prediction > 0 else "down",
        probability=sigmoid(prediction) * confidence_adjustment,
        expected_move=abs(prediction) * 100 * (crypto_volatility / 0.2),  # Scale by vol
        confidence_interval=calculate_wider_ci(prediction, crypto_volatility)
    )
```

**Crypto-Specific Technical Indicators:**

| Indicator | Parameters | Purpose |
|-----------|------------|---------|
| **BTC Correlation** | 30-day rolling | Market beta |
| **Futures Basis** | Perpetual vs spot | Contango/backwardation |
| **Funding Rate** | 8-hour | Sentiment, carry |
| **Open Interest** | Total perp OI | Leverage in system |
| **Liquidation Heatmap** | Long/short levels | Support/resistance |
| **Order Book Imbalance** | Level 2 data | Short-term pressure |
| **Exchange Flow** | In/out flows | Accumulation/distribution |

---

## Crypto Risk Management

### 1. Enhanced Volatility Model

```python
def assess_crypto_risk(returns: np.ndarray,
                       btc_returns: np.ndarray) -> CryptoRiskMetrics:
    """
    Crypto-specific risk assessment.
    Based on: GARCH-GRU (2504.09380), adapted for crypto
    """
    # Base volatility (GARCH-GRU)
    base_volatility = forecast_crypto_volatility(returns)
    
    # BTC beta
    btc_beta = calculate_beta(returns, btc_returns)
    
    # Crypto-specific adjustments
    liquidity_score = get_liquidity_score(symbol)  # 0-1
    exchange_concentration = get_exchange_concentration(symbol)  # Herfindahl index
    
    # Adjust volatility
    if liquidity_score < 0.5:
        base_volatility *= 1.5  # Illiquid → higher vol
    if exchange_concentration > 0.5:
        base_volatility *= 1.3  # Concentrated → higher vol
    
    # Calculate VaR (wider for crypto)
    var_95 = base_volatility * norm.ppf(0.05) * 1.5  # 50% wider than stocks
    cvar_95 = base_volatility * 2.5  # CVaR multiplier for fat tails
    
    # Determine regime (crypto thresholds higher)
    if base_volatility > 1.0:  # >100% annual vol
        regime = "crisis"
    elif base_volatility > 0.7:
        regime = "elevated"
    elif base_volatility > 0.4:
        regime = "normal"
    else:
        regime = "calm"
    
    # Position limits (stricter than stocks)
    position_limits = {
        'crisis': 0.02,    # 2% max
        'elevated': 0.05,  # 5% max
        'normal': 0.10,    # 10% max
        'calm': 0.15       # 15% max
    }
    
    # Leverage limits (crypto-specific)
    leverage_limits = {
        'crisis': 1.0,     # No leverage
        'elevated': 2.0,   # 2x max
        'normal': 3.0,     # 3x max
        'calm': 5.0        # 5x max
    }
    
    return CryptoRiskMetrics(
        current_volatility=base_volatility,
        btc_beta=btc_beta,
        var_95=var_95,
        cvar_95=cvar_95,
        risk_regime=regime,
        position_limit=position_limits[regime],
        leverage_limit=leverage_limits[regime],
        liquidity_score=liquidity_score
    )
```

### 2. Exchange Risk Diversification

```python
class ExchangeRiskManager:
    def __init__(self):
        self.max_per_exchange = 0.30  # 30% max on single exchange
        self.preferred_exchanges = ['binance', 'coinbase', 'kraken']
        self.dex_allocation = 0.10  # 10% on DEX
        
    def allocate_across_exchanges(self, total_position: float,
                                   symbol: str) -> List[ExchangeAllocation]:
        """
        Split position across exchanges to reduce counterparty risk.
        """
        allocations = []
        remaining = total_position
        
        # DEX allocation (10%)
        dex_amount = total_position * self.dex_allocation
        allocations.append(ExchangeAllocation(
            exchange='uniswap' if symbol.startswith('ETH') else 'pancakeswap',
            amount=dex_amount,
            type='DEX'
        ))
        remaining -= dex_amount
        
        # CEX allocation (90%)
        num_cex = len(self.preferred_exchanges)
        per_cex = remaining / num_cex
        
        for exchange in self.preferred_exchanges:
            allocations.append(ExchangeAllocation(
                exchange=exchange,
                amount=per_cex,
                type='CEX'
            ))
        
        return allocations
```

**Exchange Allocation:**

| Exchange | Allocation | Purpose |
|----------|------------|---------|
| **Binance** | 30% | Liquidity, altcoins |
| **Coinbase** | 30% | USD on/off-ramp, safety |
| **Kraken** | 30% | Futures, margin |
| **Uniswap/Pancake** | 10% | DEX, new tokens |

---

### 3. Crypto Circuit Breakers

```python
class CryptoCircuitBreaker:
    def __init__(self):
        self.daily_loss_limit = 0.08  # 8% (higher than stocks)
        self.max_drawdown = 0.15  # 15% (crypto more volatile)
        self.liquidation_threshold = 0.20  # 20% from entry
        self.funding_spike_threshold = 0.01  # 1% per 8h
        
    def check(self, position: Position, 
              current_price: float,
              funding_rate: float) -> bool:
        """
        Crypto-specific circuit breakers.
        """
        # Daily loss
        if position.daily_pnl < -self.daily_loss_limit:
            logger.warning("Daily loss limit breached")
            return True
        
        # Drawdown
        if position.drawdown > self.max_drawdown:
            logger.warning("Max drawdown breached")
            return True
        
        # Liquidation check (for leveraged positions)
        if position.leverage > 1:
            liquidation_price = position.liquidation_price
            if current_price < liquidation_price * 1.1:  # 10% buffer
                logger.warning("Approaching liquidation")
                return True
        
        # Funding spike (for perps)
        if abs(funding_rate) > self.funding_spike_threshold:
            logger.warning("Funding rate spike")
            return True
        
        return False
```

---

## Crypto Position Sizing

### Modified Kelly for Crypto

```python
def calculate_crypto_position_size(signal: CombinedSignal,
                                    risk_metrics: CryptoRiskMetrics,
                                    portfolio_value: float,
                                    funding_signal: FundingSignal) -> float:
    """
    Crypto position sizing with funding rate consideration.
    """
    # Base Kelly
    p = signal.confidence
    q = 1 - p
    b = abs(signal.raw_signal) / risk_metrics.current_volatility
    
    kelly_fraction = (p * b - q) / b if b > 0 else 0
    
    # Crypto adjustments
    kelly_fraction *= 0.5  # Half-Kelly (more conservative)
    
    # Apply position limit
    kelly_fraction = min(kelly_fraction, risk_metrics.position_limit)
    
    # Add funding carry (if positive)
    if funding_signal.expected_carry > 0.20:  # >20% APY
        carry_bonus = 0.02  # Add 2% for carry
        kelly_fraction = min(kelly_fraction + carry_bonus, 
                            risk_metrics.position_limit * 1.2)
    
    # Leverage decision
    leverage = min(risk_metrics.leverage_limit, 3.0)  # Cap at 3x default
    
    # Final position
    position_value = portfolio_value * kelly_fraction * leverage
    
    return position_value
```

**Example Crypto Position:**

| Input | Value |
|-------|-------|
| Signal | +0.35 (moderate bullish) |
| Confidence | 0.70 |
| Volatility | 0.65 (crypto normal) |
| Kelly | (0.70 × 0.54 - 0.30) / 0.54 = 0.14 |
| Half-Kelly | 0.07 |
| Position Limit (normal) | 0.10 |
| Funding Carry | +25% APY → +0.02 |
| Final Size | min(0.09, 0.12) = 0.09 |
| Leverage | 3x |
| Portfolio | $100,000 |
| **Position** | **$27,000 (27% with 3x leverage)** |

---

## Crypto Execution

### Multi-Exchange Execution

```python
def execute_crypto_order(symbol: str,
                          quantity: float,
                          side: str,
                          exchanges: List[str]) -> CryptoExecutionPlan:
    """
    Execute across multiple crypto exchanges.
    Based on: WebCryptoAgent (2601.04687)
    """
    # Get order book depth per exchange
    order_books = {}
    for exchange in exchanges:
        order_books[exchange] = get_order_book(symbol, exchange)
    
    # Calculate optimal split (minimize slippage)
    total_value = quantity * get_price(symbol)
    
    allocations = []
    remaining = quantity
    
    for exchange in exchanges:
        # Calculate how much this exchange can absorb
        depth = calculate_depth(order_books[exchange], slippage_limit=0.005)  # 50 bps
        
        allocation = min(remaining, depth)
        allocations.append({
            'exchange': exchange,
            'quantity': allocation,
            'expected_slippage': estimate_slippage(allocation, order_books[exchange])
        })
        remaining -= allocation
    
    # DEX allocation (if applicable)
    if remaining > 0 and is_erc20(symbol):
        allocations.append({
            'exchange': 'uniswap',
            'quantity': remaining,
            'expected_slippage': estimate_dex_slippage(symbol, remaining)
        })
    
    # Generate orders
    orders = []
    for alloc in allocations:
        if alloc['exchange'] in ['uniswap', 'pancakeswap']:
            # DEX: single transaction
            orders.append(CryptoOrder(
                exchange=alloc['exchange'],
                quantity=alloc['quantity'],
                type='DEX_SWAP',
                expected_slippage=alloc['expected_slippage']
            ))
        else:
            # CEX: slice like traditional
            slices = create_vwap_slices(alloc['quantity'], hours=2)
            for slice in slices:
                orders.append(CryptoOrder(
                    exchange=alloc['exchange'],
                    quantity=slice.quantity,
                    type='LIMIT' if slice.order_type == 'LIMIT' else 'MARKET',
                    price=slice.price,
                    expected_slippage=alloc['expected_slippage'] / len(slices)
                ))
    
    return CryptoExecutionPlan(orders=orders)
```

**Crypto Execution Fees:**

| Exchange | Maker | Taker | Withdrawal |
|----------|-------|-------|------------|
| **Binance** | 0.10% | 0.10% | Network fee |
| **Coinbase** | 0.40% | 0.60% | Free (ACH) |
| **Kraken** | 0.16% | 0.26% | Network fee |
| **Uniswap** | - | 0.30% + gas | Gas only |

**Optimization:** Use limit orders on CEX (maker fees), batch DEX transactions.

---

## Crypto-Specific Strategies

### 1. BTC/ETH Basis Trade

```python
def basis_trade_strategy() -> List[Trade]:
    """
    Cash-and-carry: Long spot, short perp when basis is wide.
    """
    btc_spot = get_price('BTC', 'spot')
    btc_perp = get_price('BTC', 'perpetual')
    basis = (btc_perp - btc_spot) / btc_spot
    annualized_basis = basis * 365  # Perps are perpetual
    
    if annualized_basis > 0.15:  # >15% annualized
        # Short perp, long spot
        return [
            Trade(symbol='BTC', side='buy', size=1.0, market='spot'),
            Trade(symbol='BTC', side='sell', size=1.0, market='perpetual', leverage=1.0)
        ]
    elif annualized_basis < -0.15:
        # Long perp, short spot (if you have BTC)
        return [
            Trade(symbol='BTC', side='sell', size=1.0, market='spot'),
            Trade(symbol='BTC', side='buy', size=1.0, market='perpetual', leverage=1.0)
        ]
    else:
        return []  # No trade
```

**Expected Return:** 10-20% APY (market-dependent)  
**Risk:** Funding rate turns against you

---

### 2. Funding Rate Arbitrage

```python
def funding_arbitrage_strategy() -> List[Trade]:
    """
    Long on exchange with low/negative funding,
    Short on exchange with high funding.
    """
    exchanges = ['binance', 'bybit', 'okx']
    funding_rates = {ex: get_funding_rate('BTC', ex) for ex in exchanges}
    
    max_funding = max(funding_rates.items(), key=lambda x: x[1])
    min_funding = min(funding_rates.items(), key=lambda x: x[1])
    
    spread = max_funding[1] - min_funding[1]
    annualized_spread = spread * 3 * 365  # 8-hour funding
    
    if annualized_spread > 0.30:  # >30% APY
        return [
            Trade(symbol='BTC', side='buy', size=1.0, 
                  exchange=min_funding[0], market='perpetual'),
            Trade(symbol='BTC', side='sell', size=1.0,
                  exchange=max_funding[0], market='perpetual')
        ]
    else:
        return []
```

**Expected Return:** 20-50% APY (when dislocations occur)  
**Risk:** Exchange risk, funding convergence

---

### 3. Altcoin Momentum (Top 50)

```python
def altcoin_momentum_strategy() -> List[Trade]:
    """
    Long top gainers (24h), short top losers (mean reversion).
    Based on: Crypto momentum research
    """
    # Get top 50 altcoins by market cap
    altcoins = get_top_altcoins(50)
    
    # Calculate 24h returns
    returns_24h = {coin: get_return(coin, '24h') for coin in altcoins}
    
    # Sort
    top_gainers = sorted(returns_24h.items(), key=lambda x: x[1], reverse=True)[:5]
    top_losers = sorted(returns_24h.items(), key=lambda x: x[1])[:5]
    
    trades = []
    
    # Long gainers (momentum)
    for coin, ret in top_gainers:
        if ret > 0.10:  # >10% in 24h
            trades.append(Trade(symbol=coin, side='buy', size=0.02))  # 2% each
    
    # Short losers (mean reversion)
    for coin, ret in top_losers:
        if ret < -0.10:  # <-10% in 24h
            trades.append(Trade(symbol=coin, side='sell', size=0.02, leverage=2.0))
    
    return trades
```

**Expected Return:** 5-15% monthly  
**Risk:** High volatility, liquidations

---

## Crypto Performance Metrics

### Crypto-Specific KPIs

```python
@dataclass
class CryptoPerformanceMetrics:
    # Returns
    total_return: float
    annualized_return: float
    btc_alpha: float  # Return vs BTC
    
    # Risk-adjusted
    sharpe_ratio: float
    sortino_ratio: float
    calmar_ratio: float
    
    # Crypto-specific
    funding_income: float  # From perp funding
    staking_yield: float  # From staked assets
    airdrop_income: float  # From airdrops
    
    # Risk
    max_drawdown: float
    var_95: float
    btc_correlation: float
    liquidation_count: int
    
    # Trading
    win_rate: float
    avg_holding_period: timedelta
    turnover_rate: float
    slippage_bps: float
```

**Crypto Benchmarks:**

| Metric | Good | Excellent |
|--------|------|-----------|
| **Annual Return** | >30% | >100% |
| **Sharpe Ratio** | >1.0 | >2.0 |
| **BTC Alpha** | >5% | >20% |
| **Max Drawdown** | <20% | <10% |
| **Funding Income** | >5% APY | >15% APY |
| **Win Rate** | >50% | >60% |

---

## Summary: Crypto vs Traditional

| Aspect | Traditional | Crypto Adaptation |
|--------|-------------|-------------------|
| **Signal Sources** | Fundamentals, technicals | +On-chain, social, funding |
| **Volatility** | 15-25% | 60-100% (adjust position sizes) |
| **Position Limits** | 10% max | 5% max (crypto) |
| **Leverage** | 2-4x | 3-5x (crypto, capped) |
| **Execution** | Single broker | Multi-exchange + DEX |
| **Risk Metrics** | VaR, Sharpe | +Liquidation risk, funding |
| **Circuit Breakers** | 5% daily | 8% daily (crypto) |
| **Monitoring** | Market hours | 24/7 (3 shifts) |
| **Idle Capital** | Cash (0%) | Stablecoin yield (5-10% APY) |
| **Alpha Sources** | Stock picking | +Basis trades, funding arb, airdrops |

---

**Document Status:** Ready for crypto implementation  
**Version:** 1.0 (Crypto)  
**Markets:** BTC, ETH, Top 50 Altcoins  
**Next:** Code implementation for crypto agents
