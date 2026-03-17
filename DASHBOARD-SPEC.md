# CryptoAI & CryptoWatch - Dashboard Specification

**Comprehensive Trading & Surveillance Dashboard**

**Version:** 1.0  
**Date:** March 18, 2026  
**Status:** Ready for Development  
**Author:** Riza with Emmilia Help

---

## Executive Summary

**Purpose:** Unified dashboard for monitoring CryptoAI trading system + CryptoWatch market surveillance

**Users:**
- **Retail Traders** (Free/Premium tiers)
- **Institutional Clients** ($5K/month)
- **Trading Team** (internal)
- **Compliance Officers** (regulatory reporting)

**Platforms:**
- **Web Dashboard** (React/Next.js - production)
- **Mobile App** (React Native - Phase 2)
- **Streamlit MVP** (2-3 weeks - immediate)

---

## 📊 Dashboard Architecture

### High-Level Design

```
┌─────────────────────────────────────────────────────────────┐
│ PRESENTATION LAYER                                          │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ Web (React) │ │ Mobile (RN) │ │ Streamlit   │            │
│ │             │ │             │ │ (MVP)       │            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ API GATEWAY (FastAPI + WebSocket)                           │
│ - Authentication (JWT)                                      │
│ - Rate limiting                                             │
│ - Real-time streaming                                       │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ DATA AGGREGATION LAYER                                      │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│ │ Trading     │ │ Surveillance│ │ Risk        │            │
│ │ Data        │ │ Data        │ │ Data        │            │
│ └─────────────┘ └─────────────┘ └─────────────┘            │
└──────────────────┬──────────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────────┐
│ DATABASE LAYER                                              │
│ - PostgreSQL (relational data)                              │
│ - TimescaleDB (time-series)                                 │
│ - Redis (real-time cache)                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎨 Dashboard Layout (Production - React)

### 1. **Main Dashboard** (Default View)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ HEADER                                                                  │
│ [Logo] CryptoAI/CryptoWatch  [Portfolio: $523,450]  [User]  [Logout]   │
└─────────────────────────────────────────────────────────────────────────┘

┌──────────────┬──────────────────────────────────────────┬───────────────┐
│ SIDEBAR      │ MAIN CONTENT                             │ RIGHT PANEL   │
│              │                                          │               │
│ [Dashboard]  │ ┌────────────────────────────────────┐  │ [Alerts]      │
│ [Portfolio]  │ │ PORTFOLIO OVERVIEW                 │  │               │
│ [Signals]    │ │ Total Value: $523,450 (+2.3%)      │  │ 🔴 3 Critical │
│ [Surveillance│ │ Today's P&L: +$12,340 (+2.3%)     │  │ 🟡 5 High     │
│ [Analytics]  │ │ Unrealized P&L: +$8,450           │  │ 🟢 12 Medium  │
│ [Settings]   │ │                                    │  │ ⚪ 45 Low     │
│              │ │ [Chart: Equity Curve - 7 days]     │  │               │
│ FILTERS      │ └────────────────────────────────────┘  │ [Recent       │
│              │                                          │  Trades]      │
│ [Timeframe]  │ ┌──────────────┐ ┌──────────────┐      │               │
│ 1H 4H 1D 1W  │ │ TOP POSITIONS│ │ SIGNALS      │      │ AAPL +500     │
│              │ │              │ │              │      │ BTC   +1000   │
│ [Assets]     │ │ BTC  $42,340 │ │ BUY  BTC    │      │ ETH   -200    │
│ ☑ BTC        │ │ ETH  $38,120 │ │ SELL ETH    │      │ SOL   +350    │
│ ☑ ETH        │ │ SOL  $15,670 │ │ HOLD AAPL   │      │               │
│ ☑ AAPL       │ │ AAPL $28,450 │ │              │      │               │
│              │ │              │ │              │      │               │
│ [Severity]   │ └──────────────┘ └──────────────┘      │ [System       │
│ ☑ Critical   │                                          │  Status]      │
│ ☑ High       │ ┌────────────────────────────────────┐  │               │
│ ☑ Medium     │ │ SURVEILLANCE ALERTS                │  │ ✅ API: OK   │
│ ☑ Low        │ │ 🚨 Whale: 1000 BTC moved           │  │ ✅ DB: OK    │
│              │ │ ⚠️  Pump detected: $PEPE           │  │ ✅ Redis: OK │
│              │ │ ℹ️  Volume spike: +450%            │  │ ✅ GPU: 45%  │
│              │ └────────────────────────────────────┘  │               │
└──────────────┴──────────────────────────────────────────┴───────────────┘
```

---

## 📈 Dashboard Views (Detailed)

### 2. **Portfolio View**

**Purpose:** Detailed portfolio performance & positions

**Components:**

#### A. Portfolio Summary Cards
| Metric | Value | Change |
|--------|-------|--------|
| **Total Value** | $523,450 | +2.3% |
| **Today's P&L** | +$12,340 | +2.3% |
| **Unrealized P&L** | +$8,450 | +1.6% |
| **Realized P&L** | +$3,890 | +0.7% |
| **Cash Balance** | $45,230 | - |
| **Margin Used** | $125,000 | 25% |

#### B. Equity Curve Chart
- **Type:** Line chart (Plotly/Recharts)
- **Timeframes:** 1H, 4H, 1D, 1W, 1M, ALL
- **Comparisons:** vs BTC, vs ETH, vs Benchmark
- **Annotations:** Trades, signals, events

#### C. Position Table
| Symbol | Size | Entry | Current | P&L | % | Action |
|--------|------|-------|---------|-----|---|--------|
| BTC | 1.5 | $41,200 | $42,340 | +$1,710 | +4.2% | [Close] |
| ETH | 12.0 | $3,050 | $3,180 | +$1,560 | +4.3% | [Close] |
| SOL | 150 | $98 | $104 | +$900 | +6.1% | [Close] |
| AAPL | 200 | $142 | $141 | -$200 | -1.4% | [Close] |

#### D. Allocation Charts
- **Pie Chart:** Asset allocation (BTC 35%, ETH 28%, SOL 15%, AAPL 12%, Cash 10%)
- **Bar Chart:** Sector allocation (Crypto 78%, Stocks 12%, Cash 10%)
- **Geographic:** US 45%, EU 25%, Asia 20%, Other 10%

#### E. Performance Metrics
| Metric | Value | Benchmark |
|--------|-------|-----------|
| **CAGR** | 78% | 42% (BTC) |
| **Sharpe Ratio** | 2.0 | 1.2 (BTC) |
| **Max Drawdown** | -12% | -35% (BTC) |
| **Win Rate** | 56% | 48% |
| **Profit Factor** | 2.1 | 1.5 |
| **Avg Trade** | +$1,240 | +$450 |

---

### 3. **Signals View**

**Purpose:** Monitor & analyze trading signals from all agents

**Components:**

#### A. Signal Feed (Real-time)
| Time | Agent | Symbol | Signal | Strength | Confidence | Status |
|------|-------|--------|--------|----------|------------|--------|
| 10:32 | On-Chain | BTC | BUY | Strong | 0.78 | ✅ Active |
| 10:28 | Social | ETH | BUY | Medium | 0.65 | ✅ Active |
| 10:15 | Technical | SOL | SELL | Weak | 0.55 | ⏳ Pending |
| 09:45 | Funding | BTC | BUY | Strong | 0.82 | ✅ Active |

#### B. Signal Performance
| Agent | Signals | Win Rate | Avg Return | Sharpe |
|-------|---------|----------|------------|--------|
| **On-Chain** | 245 | 58% | +2.3% | 1.8 |
| **Social** | 312 | 54% | +1.8% | 1.5 |
| **Technical** | 428 | 56% | +2.1% | 1.7 |
| **Funding** | 156 | 62% | +2.8% | 2.1 |
| **Manager** | 189 | 59% | +2.5% | 1.9 |

#### C. Signal Heatmap
```
        BTC    ETH    SOL    AAPL   TSLA
BUY     🟢     🟢     🟡     🔴     🔴
HOLD    🟡     🟡     🟢     🟢     🟡
SELL    🔴     🔴     🟡     🟡     🟢

🟢 = Strong Signal
🟡 = Medium Signal
🔴 = Weak/No Signal
```

#### D. Signal Details Modal
```
┌────────────────────────────────────────────┐
│ SIGNAL DETAILS: BTC BUY                    │
├────────────────────────────────────────────┤
│ Agent: On-Chain Analyst                    │
│ Time: 2026-03-18 10:32:45 UTC             │
│ Strength: Strong (0.78)                    │
│ Confidence: 82%                            │
│                                            │
│ RATIONALE:                                 │
│ - 1000 BTC whale accumulation detected     │
│ - Exchange outflow: +15% (24h)             │
│ - Active addresses: +8% (7d)               │
│ - Hash rate: ATH                           │
│                                            │
│ RECOMMENDED ACTION:                        │
│ - Entry: $42,000-42,500                    │
│ - Target: $45,000 (+7%)                    │
│ - Stop: $40,500 (-3.5%)                    │
│ - Position Size: 5% of portfolio           │
│                                            │
│ [Execute] [Dismiss] [Set Alert]            │
└────────────────────────────────────────────┘
```

---

### 4. **Surveillance View**

**Purpose:** Real-time market surveillance & manipulation detection

**Components:**

#### A. Live Alert Feed
| Time | Type | Severity | Symbol | Details | Action |
|------|------|----------|--------|---------|--------|
| 10:35 | Whale | 🔴 Critical | BTC | 1000 BTC moved | [View] |
| 10:28 | Pump | 🟡 High | PEPE | +450% volume | [View] |
| 10:15 | Spoof | 🟡 High | ETH | $2M spoof detected | [View] |
| 09:45 | Volume | 🟢 Medium | SOL | +120% volume | [View] |

#### B. Whale Tracker Map
- **Visualization:** Network graph (Gephi/Plotly)
- **Nodes:** Wallets, exchanges
- **Edges:** Transactions
- **Size:** Transaction volume
- **Color:** Inflow (red), Outflow (green)

#### C. Exchange Flow Chart
- **Type:** Stacked area chart
- **X-axis:** Time (24h, 7d, 30d)
- **Y-axis:** Net flow (BTC/ETH/USDT)
- **Lines:** Binance, Coinbase, Kraken, etc.

#### D. Manipulation Detection
| Pattern | Detected | Last 24h | Last 7d |
|---------|----------|----------|---------|
| **Spoofing** | 3 | 12 | 45 |
| **Wash Trading** | 1 | 5 | 23 |
| **Pump & Dump** | 2 | 8 | 34 |
| **Front-Running** | 5 | 18 | 67 |

#### E. Social Sentiment Gauge
```
        BTC Sentiment
        ┌─────────┐
        │    🟢   │
 FUD ←──┤  0.72   ├──→ FOMO
        │ BULLISH │
        └─────────┘
        
Influencer: Bullish (65%)
Retail: Bullish (58%)
Volume: High (+45%)
```

---

### 5. **Analytics View**

**Purpose:** Deep-dive analytics & reporting

**Components:**

#### A. Performance Attribution
| Factor | Contribution |
|--------|--------------|
| **On-Chain Signals** | +12.3% |
| **Social Sentiment** | +8.7% |
| **Technical Analysis** | +10.2% |
| **Funding Arbitrage** | +6.5% |
| **Staking Yield** | +2.1% |
| **Total** | +39.8% |

#### B. Risk Metrics
| Metric | Current | Limit | Status |
|--------|---------|-------|--------|
| **VaR (95%)** | -2.3% | -5% | ✅ OK |
| **CVaR (95%)** | -3.8% | -8% | ✅ OK |
| **Max DD** | -12% | -15% | ✅ OK |
| **Leverage** | 2.5x | 3x | ✅ OK |
| **Concentration** | 35% | 40% | ✅ OK |

#### C. Correlation Matrix
```
       BTC    ETH    SOL    AAPL
BTC    1.00   0.85   0.72   0.35
ETH    0.85   1.00   0.78   0.32
SOL    0.72   0.78   1.00   0.28
AAPL   0.35   0.32   0.28   1.00
```

#### D. Trade History Table
| Date | Symbol | Side | Size | Entry | Exit | P&L | Status |
|------|--------|------|------|-------|------|-----|--------|
| Mar 17 | BTC | LONG | 1.5 | $41,200 | $42,800 | +$2,400 | ✅ Closed |
| Mar 16 | ETH | SHORT | 10 | $3,150 | $3,080 | +$700 | ✅ Closed |
| Mar 15 | SOL | LONG | 200 | $98 | $104 | +$1,200 | ✅ Closed |

#### E. Custom Reports
- **Daily P&L Report** (auto-generated, 8 AM UTC)
- **Weekly Performance** (every Monday)
- **Monthly Investor Report** (1st of month)
- **Compliance Report** (quarterly, institutional)

---

### 6. **Settings View**

**Purpose:** User configuration & preferences

**Components:**

#### A. Alert Configuration
| Alert Type | Channels | Threshold |
|------------|----------|-----------|
| **Whale Transactions** | Telegram, Email | >500 BTC |
| **Price Alerts** | SMS, Push | ±5% |
| **Signal Alerts** | Telegram | Confidence >0.7 |
| **Risk Alerts** | SMS, Email | VaR breach |

#### B. API Keys Management
| Exchange | Status | Last Used | Action |
|----------|--------|-----------|--------|
| Binance | ✅ Active | 2 min ago | [Revoke] |
| Coinbase | ✅ Active | 1 hour ago | [Revoke] |
| Kraken | ⚠️ Expired | 30 days ago | [Renew] |

#### C. Subscription Tier
```
┌────────────────────────────────────────┐
│ CURRENT PLAN: PREMIUM ($99/month)      │
├────────────────────────────────────────┤
│ ✅ Real-time alerts                    │
│ ✅ Advanced analytics                  │
│ ✅ API access (1K calls/month)         │
│ ✅ Historical data (1 year)            │
│ ❌ Compliance reporting                │
│ ❌ Dedicated support                   │
│                                        │
│ [Upgrade to Institutional - $5K/mo]    │
└────────────────────────────────────────┘
```

#### D. Two-Factor Authentication
- [ ] Enable 2FA
- [ ] SMS backup
- [ ] Backup codes

---

## 🛠️ Technology Stack

### MVP (Streamlit - 2-3 weeks)

```python
# Core Libraries
streamlit==1.31.0          # Dashboard framework
plotly==5.19.0             # Interactive charts
pandas==2.2.0              # Data manipulation
redis==5.0.1               # Real-time data
sqlalchemy==2.0.25         # Database access
```

**File Structure:**
```
cryptowatch/dashboard/
├── streamlit_app.py          # Main app
├── components/
│   ├── portfolio.py          # Portfolio components
│   ├── signals.py            # Signal components
│   ├── surveillance.py       # Surveillance components
│   └── analytics.py          # Analytics components
├── utils/
│   ├── data_loader.py        # Data fetching
│   └── formatters.py         # Data formatting
└── static/
    └── logo.png              # Assets
```

**Deployment:**
```bash
# Run locally
streamlit run cryptowatch/dashboard/streamlit_app.py

# Deploy to Streamlit Cloud
streamlit cloud deploy
```

---

### Production (React/Next.js - 8-12 weeks)

```javascript
// Core Dependencies
{
  "next": "^14.1.0",           // React framework
  "react": "^18.2.0",          // UI library
  "recharts": "^2.10.0",       // Charts
  "plotly.js": "^2.29.0",      // Advanced charts
  "socket.io-client": "^4.6.0", // WebSocket
  "tailwindcss": "^3.4.0",     // Styling
  "zustand": "^4.5.0",         // State management
  "axios": "^1.6.0",           // HTTP client
  "date-fns": "^3.0.0"         // Date utilities
}
```

**File Structure:**
```
dashboard-web/
├── src/
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   ├── Portfolio/
│   │   ├── Signals/
│   │   ├── Surveillance/
│   │   └── Analytics/
│   ├── pages/
│   │   ├── index.tsx          # Main dashboard
│   │   ├── portfolio.tsx
│   │   ├── signals.tsx
│   │   ├── surveillance.tsx
│   │   └── analytics.tsx
│   ├── hooks/
│   │   ├── useWebSocket.ts
│   │   ├── usePortfolio.ts
│   │   └── useSignals.ts
│   ├── utils/
│   │   ├── api.ts
│   │   └── formatters.ts
│   └── styles/
│       └── globals.css
├── public/
└── package.json
```

---

## 📱 Mobile App (Phase 2)

**Framework:** React Native

**Key Screens:**
1. **Home:** Portfolio summary + alerts
2. **Portfolio:** Positions + P&L
3. **Signals:** Signal feed + details
4. **Surveillance:** Alert feed
5. **Settings:** Configuration

**Features:**
- Push notifications (critical alerts)
- Biometric authentication
- Offline mode (cached data)
- Touch ID / Face ID

---

## 🎨 Design System

### Color Palette
```
Primary:   #3B82F6 (Blue)
Success:   #10B981 (Green)
Warning:   #F59E0B (Orange)
Danger:    #EF4444 (Red)
Background: #0F172A (Dark)
Card:      #1E293B (Dark gray)
Text:      #F8FAFC (Light)
```

### Typography
- **Headings:** Inter (Bold)
- **Body:** Inter (Regular)
- **Monospace:** JetBrains Mono (for numbers)

### Component Library
- **Buttons:** Primary, Secondary, Danger
- **Cards:** Standard, Elevated, Collapsible
- **Tables:** Sortable, Filterable, Paginated
- **Charts:** Line, Bar, Pie, Heatmap, Candlestick

---

## 📊 Real-Time Data Flow

```
┌─────────────┐     WebSocket      ┌─────────────┐
│   Backend   │ ──────────────────► │  Frontend   │
│  (FastAPI)  │    JSON Messages    │  (React)    │
└─────────────┘                     └─────────────┘
       │                                   │
       │                                   │
       ▼                                   ▼
┌─────────────┐                     ┌─────────────┐
│  PostgreSQL │                     │   State     │
│  TimescaleDB│                     │  (Zustand)  │
└─────────────┘                     └─────────────┘
```

**Message Format:**
```json
{
  "type": "signal_update",
  "timestamp": "2026-03-18T10:32:45Z",
  "data": {
    "agent": "on_chain",
    "symbol": "BTC",
    "signal": "BUY",
    "strength": 0.78,
    "confidence": 0.82
  }
}
```

---

## 🔐 Security

### Authentication
- JWT tokens (15-minute expiry)
- Refresh tokens (7-day expiry)
- 2FA (TOTP)
- Session management

### Authorization
- Role-based access control (RBAC)
- API key scoping
- IP whitelisting (institutional)

### Data Protection
- HTTPS everywhere
- Encrypted database (AES-256)
- Secrets management (HashiCorp Vault)
- Audit logging

---

## 📈 Performance Targets

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Page Load Time** | <2 seconds | Lighthouse |
| **WebSocket Latency** | <100ms | Backend → Frontend |
| **Chart Render Time** | <500ms | Plotly/Recharts |
| **API Response Time** | <200ms (p95) | FastAPI |
| **Uptime** | 99.9% | Monitoring |

---

## 📋 Development Phases

### Phase 1: MVP (Streamlit) - 2-3 weeks
- [ ] Portfolio overview
- [ ] Signal feed
- [ ] Basic alerts
- [ ] Exchange flow chart
- [ ] Deploy to production

**Budget:** $15,000  
**Team:** 1 Backend + 1 Frontend

### Phase 2: Production Web (React) - 8-12 weeks
- [ ] All dashboard views
- [ ] Real-time WebSocket
- [ ] Advanced analytics
- [ ] Custom reports
- [ ] Mobile-responsive

**Budget:** $50,000  
**Team:** 2 Frontend + 1 Backend + 1 Designer

### Phase 3: Mobile App (React Native) - 6-8 weeks
- [ ] iOS app
- [ ] Android app
- [ ] Push notifications
- [ ] Offline mode

**Budget:** $40,000  
**Team:** 2 Mobile + 1 Backend

---

## 💰 Cost Estimate

| Phase | Duration | Budget |
|-------|----------|--------|
| **MVP (Streamlit)** | 3 weeks | $15,000 |
| **Production Web** | 10 weeks | $50,000 |
| **Mobile App** | 7 weeks | $40,000 |
| **Total** | 20 weeks | **$105,000** |

**Ongoing Costs:**
- Hosting (Vercel/AWS): $500/month
- Data APIs: $2,000/month
- Monitoring: $200/month
- **Total:** $2,700/month

---

## 🎯 Success Metrics

| Metric | Target | Timeline |
|--------|--------|----------|
| **Daily Active Users** | 1,000 | Month 3 |
| **Session Duration** | >5 minutes | Month 3 |
| **User Retention (7d)** | >60% | Month 3 |
| **NPS** | >50 | Month 6 |
| **Page Load Time** | <2 seconds | Month 1 |

---

## 📞 Next Steps

**Week 1:**
1. [ ] Finalize dashboard mockups (Figma)
2. [ ] Setup Streamlit environment
3. [ ] Create database views
4. [ ] Build portfolio component

**Week 2:**
1. [ ] Build signal feed component
2. [ ] Build surveillance alerts
3. [ ] Integrate real-time data (WebSocket)
4. [ ] Deploy MVP to staging

**Week 3:**
1. [ ] User testing (10 beta users)
2. [ ] Bug fixes + iterations
3. [ ] Deploy to production
4. [ ] Monitor + optimize

---

**© 2026 - Riza with Emmilia Help**  
*Built with courage, calculation, and AI collaboration*

**Status:** Ready for Development  
**Next:** Create Figma mockups → Start Streamlit MVP
