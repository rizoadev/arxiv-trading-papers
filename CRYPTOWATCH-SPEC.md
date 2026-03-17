# CryptoWatch Technical Specification
## Market Surveillance System - Technical Implementation Guide

**Version:** 1.0  
**Date:** March 17, 2026  
**Based on:** CryptoAI Trading System infrastructure  
**Status:** Ready for MVP Development

---

## 1. System Architecture

### 1.1 High-Level Design

```
┌─────────────────────────────────────────────────────────────┐
│ DATA INGESTION LAYER (24/7)                                 │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ Exchange    │ │ On-Chain    │ │ Social      │            │
│ │ - Binance   │ │ - Ethereum  │ │ - Twitter   │            │
│ │ - Coinbase  │ │ - Bitcoin   │ │ - Reddit    │            │
│ │ - Kraken    │ │ - Solana    │ │ - Telegram  │            │
│ │ - 10+ more  │ │ - 5+ chains │ │ - YouTube   │            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ STREAM PROCESSING (Apache Kafka / Redis Streams)           │
│ - Real-time event streaming                                 │
│ - Event deduplication                                       │
│ - Rate limiting                                             │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ DETECTION ENGINE                                            │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ Anomaly     │ │ Manipulation│ │ Risk        │            │
│ │ Detection   │ │ Detection   │ │ Monitoring  │            │
│ │ (ML-based)  │ │ (Rule-based)│ │ (VaR, DD)   │            │
│ │ LSTM-GNN    │ │ Spoofing    │ │ Portfolio   │            │
│ │             │ │ Wash trading│ │ Exposure    │            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ ALERT SYSTEM                                                │
│ - Telegram Bot                                              │
│ - Slack Integration                                         │
│ - SMS (Twilio)                                              │
│ - Email (SendGrid)                                          │
│ - Webhook (for institutions)                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Data Ingestion

### 2.1 Exchange Data

**Supported Exchanges (MVP):**
- Binance (primary)
- Coinbase Pro
- Kraken
- Bybit
- OKX

**Data Types:**
```python
@dataclass
class ExchangeData:
    # OHLCV
    symbol: str
    timestamp: datetime
    open: float
    high: float
    low: float
    close: float
    volume: float
    
    # Order Book (L2)
    bids: List[Tuple[float, float]]  # (price, size)
    asks: List[Tuple[float, float]]
    
    # Trades
    trades: List[Trade]
    
    # Funding Rates (perps)
    funding_rate: Optional[float]
```

**Implementation:**
```python
import ccxt

class ExchangeDataIngestion:
    def __init__(self):
        self.exchanges = {
            'binance': ccxt.binance(),
            'coinbase': ccxt.coinbasepro(),
            'kraken': ccxt.kraken(),
        }
    
    async def stream_orderbook(self, symbol: str, exchange: str):
        ex = self.exchanges[exchange]
        while True:
            orderbook = ex.fetch_order_book(symbol)
            await self.process_orderbook(orderbook, exchange, symbol)
            await asyncio.sleep(1)  # 1-second updates
```

---

### 2.2 On-Chain Data

**Supported Chains (MVP):**
- Ethereum (ERC-20 tokens)
- Bitcoin
- Solana

**Data Sources:**
- Glassnode API (paid)
- Etherscan API (free tier)
- Web3.py (direct node connection)
- Dune Analytics (custom queries)

**Whale Transaction Detection:**
```python
@dataclass
class WhaleTransaction:
    hash: str
    timestamp: datetime
    from_address: str
    to_address: str
    amount: float
    symbol: str
    usd_value: float
    is_exchange_deposit: bool
    is_exchange_withdrawal: bool

class OnChainIngestion:
    def __init__(self):
        self.whale_thresholds = {
            'BTC': 1000,      # 1000 BTC
            'ETH': 10000,     # 10,000 ETH
            'USDT': 10_000_000,  # $10M USDT
        }
        self.known_exchanges = self.load_exchange_addresses()
    
    async def monitor_mempool(self, chain: str):
        # Monitor pending transactions
        async for txn in web3.eth.subscribe('pendingTransactions'):
            if self.is_whale_transaction(txn):
                await self.process_whale_txn(txn)
    
    def is_whale_transaction(self, txn) -> bool:
        return txn.value >= self.whale_thresholds.get(txn.symbol, 0)
```

---

### 2.3 Social Data

**Data Sources:**
- Twitter API v2 (paid: $5,000/month for enterprise)
- Reddit API (free)
- Telegram API (free, but need to join groups)
- YouTube Data API (free tier)

**Influencer Tracking List:**
```python
INFLUENCERS = {
    'cz_binance': {'followers': 8_000_000, 'impact': 'high'},
    'VitalikButerin': {'followers': 5_000_000, 'impact': 'high'},
    'michael_saylor': {'followers': 3_000_000, 'impact': 'medium'},
    'APompliano': {'followers': 1_500_000, 'impact': 'medium'},
    # ... 50+ more
}
```

**Pump Group Detection:**
```python
class SocialSurveillance:
    def __init__(self):
        self.pump_keywords = [
            'moon', 'lambo', '100x', 'pump', 'buy now',
            'dont miss out', 'life changing', 'rocket'
        ]
    
    async def monitor_telegram_groups(self):
        # Join known pump groups
        groups = ['pump_signal_1', 'crypto_moon_shots', ...]
        
        for group in groups:
            async for message in self.listen_to_group(group):
                if self.detect_coordinated_pump(message):
                    await self.alert_pump_scheme(message)
    
    def detect_coordinated_pump(self, messages: List[Message]) -> bool:
        # Look for similar messages within short time window
        keyword_count = sum(
            1 for msg in messages 
            if any(kw in msg.text for kw in self.pump_keywords)
        )
        return keyword_count > 10  # 10+ messages in 1 minute
```

---

## 3. Detection Engine

### 3.1 Anomaly Detection (ML-Based)

**LSTM-GNN for Unusual Patterns:**
```python
class AnomalyDetector:
    def __init__(self):
        # Reuse LSTM-GNN from CryptoAI
        self.model = LSTMGNNTechnicalAnalyst.load("models/lstm_gnn.pth")
    
    def detect_volume_anomaly(self, symbol: str, current_volume: float, 
                             historical_volumes: np.ndarray) -> Alert:
        # Prepare features
        features = self.prepare_features(current_volume, historical_volumes)
        
        # Predict expected volume
        expected_volume = self.model.predict(features)
        
        # Calculate deviation
        deviation = (current_volume - expected_volume) / expected_volume
        
        if abs(deviation) > 5.0:  # 500% deviation
            return Alert(
                severity='high',
                type='volume_anomaly',
                message=f"Volume anomaly: {deviation:.1f}x expected",
                symbol=symbol
            )
```

**Statistical Methods:**
```python
def detect_price_anomaly(prices: np.ndarray, window: int = 60):
    # Z-score based detection
    mean = np.mean(prices[-window:])
    std = np.std(prices[-window:])
    z_score = (prices[-1] - mean) / std
    
    if abs(z_score) > 5:  # 5 standard deviations
        return Alert(
            severity='critical',
            type='price_anomaly',
            message=f"Price anomaly: {z_score:.1f}σ from mean"
        )
```

---

### 3.2 Manipulation Detection (Rule-Based)

**Spoofing Detection:**
```python
class SpoofingDetector:
    def __init__(self):
        self.order_history = {}  # Track order lifecycle
    
    def detect_spoofing(self, order_book: OrderBook) -> List[Alert]:
        alerts = []
        
        # Look for large orders (>100 BTC equivalent)
        large_orders = [o for o in order_book.bids + order_book.asks 
                       if o.size * o.price > 1_000_000]  # $1M+
        
        for order in large_orders:
            # Track time on book
            if order.id not in self.order_history:
                self.order_history[order.id] = datetime.now()
            
            time_on_book = datetime.now() - self.order_history[order.id]
            
            # If order cancels within 60 seconds without filling
            if time_on_book < timedelta(seconds=60) and not order.filled:
                alerts.append(Alert(
                    severity='medium',
                    type='spoofing',
                    message=f"Potential spoof: ${order.size * order.price:,.0f}",
                    exchange=order.exchange,
                    symbol=order.symbol
                ))
        
        return alerts
```

**Wash Trading Detection:**
```python
def detect_wash_trading(trades: List[Trade]) -> List[Alert]:
    # Look for circular trading patterns
    alerts = []
    
    # Group trades by time window (1 minute)
    time_windows = group_by_minute(trades)
    
    for window in time_windows:
        # Check if same entity is buying and selling
        buy_addresses = set(t.buyer_address for t in window)
        sell_addresses = set(t.seller_address for t in window)
        
        # If overlap, potential wash trading
        overlap = buy_addresses.intersection(sell_addresses)
        if overlap:
            alerts.append(Alert(
                severity='high',
                type='wash_trading',
                message=f"Wash trading detected: {len(overlap)} addresses"
            ))
    
    return alerts
```

---

### 3.3 Risk Monitoring

**Portfolio VaR Monitoring:**
```python
class RiskMonitor:
    def __init__(self, portfolio: Portfolio):
        self.portfolio = portfolio
    
    def calculate_var(self, confidence: float = 0.95) -> float:
        # Historical VaR
        returns = self.portfolio.historical_returns(days=90)
        var = np.percentile(returns, (1 - confidence) * 100)
        return abs(var)
    
    def check_var_breach(self) -> Alert:
        current_var = self.calculate_var()
        limit = self.portfolio.var_limit
        
        if current_var > limit:
            return Alert(
                severity='high',
                type='var_breach',
                message=f"VaR breach: {current_var:.2%} > {limit:.2%} limit"
            )
```

**Correlation Breakdown Detection:**
```python
def detect_correlation_breakdown(assets: List[str], 
                                 window: int = 30) -> List[Alert]:
    # Calculate rolling correlations
    returns = get_returns(assets, window)
    current_corr = returns.corr()
    
    # Compare to historical correlation
    historical_corr = get_historical_correlation(assets, lookback=90)
    
    # Detect significant changes
    corr_change = abs(current_corr - historical_corr)
    
    alerts = []
    if corr_change.max() > 0.5:  # 50% change in correlation
        alerts.append(Alert(
            severity='medium',
            type='correlation_breakdown',
            message=f"Correlation breakdown detected"
        ))
    
    return alerts
```

---

## 4. Alert System

### 4.1 Alert Severity Levels

```python
class AlertSeverity(Enum):
    LOW = 1       # Informational
    MEDIUM = 2    # Worth noting
    HIGH = 3      # Action required
    CRITICAL = 4  # Immediate action required
```

### 4.2 Alert Channels

**Telegram Bot:**
```python
class TelegramAlerter:
    def __init__(self, bot_token: str):
        self.bot = telegram.Bot(token=bot_token)
    
    async def send_alert(self, alert: Alert, chat_ids: List[str]):
        message = self.format_alert(alert)
        
        for chat_id in chat_ids:
            await self.bot.send_message(
                chat_id=chat_id,
                text=message,
                parse_mode='Markdown'
            )
    
    def format_alert(self, alert: Alert) -> str:
        emoji = {
            AlertSeverity.LOW: 'ℹ️',
            AlertSeverity.MEDIUM: '⚠️',
            AlertSeverity.HIGH: '🚨',
            AlertSeverity.CRITICAL: '🔴'
        }
        
        return f"""
{emoji[alert.severity]} *{alert.type.upper()}*

Severity: {alert.severity.name}
Message: {alert.message}
Time: {alert.timestamp}
        """
```

**Slack Integration:**
```python
class SlackAlerter:
    def __init__(self, webhook_url: str):
        self.webhook_url = webhook_url
    
    async def send_alert(self, alert: Alert, channels: List[str]):
        payload = {
            "text": f"🚨 {alert.type}",
            "attachments": [
                {
                    "color": self.get_color(alert.severity),
                    "fields": [
                        {"title": "Severity", "value": alert.severity.name, "short": True},
                        {"title": "Message", "value": alert.message, "short": False},
                        {"title": "Time", "value": str(alert.timestamp), "short": True}
                    ]
                }
            ]
        }
        
        requests.post(self.webhook_url, json=payload)
```

**SMS (Twilio):**
```python
class SMSAlerter:
    def __init__(self, twilio_sid: str, twilio_token: str):
        self.client = TwilioClient(twilio_sid, twilio_token)
    
    async def send_alert(self, alert: Alert, phone_numbers: List[str]):
        # Only for HIGH and CRITICAL alerts
        if alert.severity not in [AlertSeverity.HIGH, AlertSeverity.CRITICAL]:
            return
        
        message = f"🚨 {alert.type}: {alert.message}"
        
        for phone in phone_numbers:
            self.client.messages.create(
                body=message,
                from_='+1234567890',
                to=phone
            )
```

---

## 5. Dashboard

### 5.1 MVP Dashboard (Streamlit)

```python
# app.py
import streamlit as st

st.title("🔍 CryptoWatch - Market Surveillance")

# Sidebar filters
selected_severity = st.sidebar.multiselect(
    "Severity",
    ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
)

selected_type = st.sidebar.multiselect(
    "Alert Type",
    ["whale_transaction", "spoofing", "volume_spike", ...]
)

# Main dashboard
col1, col2, col3 = st.columns(3)

with col1:
    st.metric("Alerts (24h)", get_alert_count_24h())

with col2:
    st.metric("Critical Alerts", get_critical_count())

with col3:
    st.metric("Whale Movements", get_whale_count())

# Alert table
alerts_df = get_alerts(severity=selected_severity, type=selected_type)
st.dataframe(alerts_df)

# Real-time whale tracker
st.subheader("🐋 Live Whale Tracker")
whale_alerts = get_whale_alerts(limit=10)
for alert in whale_alerts:
    st.write(f"{alert.amount} {alert.symbol} - {alert.from_address[:10]}... → {alert.to_address[:10]}...")
```

### 5.2 Production Dashboard (React/Next.js)

**Features:**
- Real-time alert feed (WebSocket)
- Interactive charts (TradingView integration)
- Custom alert configuration
- Historical data exploration
- Portfolio risk dashboard
- Compliance reports

---

## 6. Database Schema

### 6.1 Tables

```sql
-- Alerts table
CREATE TABLE alerts (
    id UUID PRIMARY KEY,
    timestamp TIMESTAMPTZ NOT NULL,
    severity VARCHAR(20) NOT NULL,
    type VARCHAR(50) NOT NULL,
    message TEXT NOT NULL,
    symbol VARCHAR(20),
    exchange VARCHAR(50),
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Whale transactions
CREATE TABLE whale_transactions (
    hash VARCHAR(100) PRIMARY KEY,
    timestamp TIMESTAMPTZ NOT NULL,
    from_address VARCHAR(100),
    to_address VARCHAR(100),
    amount NUMERIC NOT NULL,
    symbol VARCHAR(20) NOT NULL,
    usd_value NUMERIC,
    is_exchange_deposit BOOLEAN,
    is_exchange_withdrawal BOOLEAN,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Exchange flows (daily aggregation)
CREATE TABLE exchange_flows (
    date DATE NOT NULL,
    exchange VARCHAR(50) NOT NULL,
    symbol VARCHAR(20) NOT NULL,
    inflow NUMERIC,
    outflow NUMERIC,
    net_flow NUMERIC,
    PRIMARY KEY (date, exchange, symbol)
);

-- User subscriptions
CREATE TABLE user_subscriptions (
    user_id UUID PRIMARY KEY,
    email VARCHAR(255),
    telegram_chat_id VARCHAR(100),
    slack_webhook VARCHAR(500),
    phone VARCHAR(20),
    tier VARCHAR(20) DEFAULT 'free',
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 7. API Design

### 7.1 Public API (REST)

```yaml
GET /api/v1/alerts
  Description: Get recent alerts
  Parameters:
    - severity: LOW|MEDIUM|HIGH|CRITICAL
    - type: string
    - symbol: string
    - limit: int (default: 100)
  Response:
    - alerts: Alert[]
    - total: int

GET /api/v1/whale-transactions
  Description: Get recent whale transactions
  Parameters:
    - symbol: string
    - min_amount: float
    - limit: int
  Response:
    - transactions: WhaleTransaction[]

POST /api/v1/alerts/subscribe
  Description: Subscribe to alerts
  Body:
    - user_id: UUID
    - alert_types: string[]
    - channels: string[] (telegram, slack, sms, email)
  Response:
    - subscription_id: UUID
```

### 7.2 WebSocket (Real-Time)

```javascript
// Client connects to WebSocket
const ws = new WebSocket('wss://api.cryptowatch.io/v1/stream');

// Subscribe to alerts
ws.send(JSON.stringify({
  action: 'subscribe',
  channel: 'alerts',
  filters: {
    severity: ['HIGH', 'CRITICAL'],
    symbols: ['BTC', 'ETH']
  }
}));

// Receive real-time alerts
ws.onmessage = (event) => {
  const alert = JSON.parse(event.data);
  console.log('New alert:', alert);
};
```

---

## 8. Infrastructure

### 8.1 Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ AWS / GCP Cloud                                             │
│                                                             │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ Web Server  │ │ API Server  │ │ Worker      │            │
│ │ (Next.js)   │ │ (FastAPI)   │ │ (Celery)    │            │
│ │             │ │             │ │             │            │
│ │ 2x t3.medium│ │ 2x t3.large │ │ 4x t3.xlarge│            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
│                                                             │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ PostgreSQL  │ │ Redis       │ │ TimescaleDB │            │
│ │ (RDS)       │ │ (ElastiCache)│ │ (Timescale) │            │
│ │             │ │             │ │             │            │
│ │ db.r5.large │ │ cache.r5.large│ │ db.r5.large │            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
│                                                             │
│ ┌─────────────┐ ┌─────────────┐                            │
│ │ GPU Server  │ │ Monitoring  │                            │
│ │ (1x L4)     │ │ (Prometheus)│                            │
│ │ g5.xlarge   │ │ + Grafana   │                            │
│ └─────────────┘ └─────────────┘                            │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 Cost Estimate (Monthly)

| Resource | Configuration | Monthly Cost |
|----------|---------------|--------------|
| **Web Server** | 2x t3.medium | $60 |
| **API Server** | 2x t3.large | $120 |
| **Workers** | 4x t3.xlarge | $240 |
| **PostgreSQL** | RDS db.r5.large | $150 |
| **Redis** | ElastiCache cache.r5.large | $130 |
| **TimescaleDB** | db.r5.large | $150 |
| **GPU Server** | g5.xlarge (1x L4) | $1,000 |
| **Data APIs** | Glassnode, Twitter, etc. | $2,000 |
| **Alert Services** | Twilio, SendGrid | $200 |
| **Total** | | **$4,050/month** |

---

## 9. Security

### 9.1 API Security

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import APIKeyHeader

API_KEY = APIKeyHeader(name="X-API-Key")

async def get_api_key(api_key: str = Depends(API_KEY)):
    if api_key not in valid_api_keys:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid API key"
        )
    return api_key
```

### 9.2 Rate Limiting

```python
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)

@app.get("/api/v1/alerts")
@limiter.limit("100/minute")
async def get_alerts(request: Request):
    ...
```

---

## 10. Testing

### 10.1 Unit Tests

```python
def test_spoofing_detection():
    detector = SpoofingDetector()
    
    # Create fake order book with spoof order
    order_book = create_order_book_with_spoof()
    
    alerts = detector.detect_spoofing(order_book)
    
    assert len(alerts) == 1
    assert alerts[0].type == 'spoofing'
```

### 10.2 Integration Tests

```python
async def test_end_to_end_alert():
    # Simulate whale transaction
    txn = create_whale_transaction(amount=1000)
    
    # Process through system
    await ingestion.process_whale_txn(txn)
    
    # Check alert was sent
    alerts = await get_recent_alerts(type='whale_transaction')
    assert len(alerts) == 1
```

---

## 11. Deployment

### 11.1 Docker Compose (Development)

```yaml
version: '3.8'

services:
  api:
    build: ./api
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/cryptowatch
      - REDIS_URL=redis://redis:6379
  
  worker:
    build: ./worker
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/cryptowatch
      - REDIS_URL=redis://redis:6379
  
  db:
    image: timescale/timescaledb:latest-pg14
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
  
  redis:
    image: redis:7-alpine
```

### 11.2 Production Deployment (Kubernetes)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cryptowatch-api
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: api
        image: cryptowatch/api:latest
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
```

---

## 12. Monitoring & Observability

### 12.1 Metrics (Prometheus)

```python
from prometheus_client import Counter, Histogram

# Alert counters
alerts_total = Counter('alerts_total', 'Total alerts', ['severity', 'type'])
alerts_processing_time = Histogram('alerts_processing_seconds', 'Alert processing time')

# Data ingestion
data_points_ingested = Counter('data_points_ingested_total', 'Data points ingested', ['source'])
```

### 12.2 Dashboards (Grafana)

**Key Panels:**
- Alerts per hour (by severity)
- Top alert types
- Data ingestion rate
- API latency (p50, p95, p99)
- Error rate
- Active users

---

## 13. Next Steps

### MVP (2-3 weeks)
1. [ ] Setup data ingestion (Binance, Ethereum)
2. [ ] Implement whale detection
3. [ ] Implement exchange flow monitoring
4. [ ] Create Telegram alert bot
5. [ ] Build Streamlit dashboard
6. [ ] Deploy to production

### Phase 2 (4-6 weeks)
1. [ ] Add order book surveillance
2. [ ] Social sentiment integration
3. [ ] Pump group detection
4. [ ] Mobile app (React Native)
5. [ ] Premium tier launch

---

**© 2026 - Riza with Emmilia Help**  
*Built with courage, calculation, and AI collaboration*
