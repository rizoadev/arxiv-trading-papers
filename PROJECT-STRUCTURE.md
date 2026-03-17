# Project Structure

**Created:** March 18, 2026  
**Projects:** CryptoAI Trading System + CryptoWatch Market Surveillance

---

## Directory Structure

```
cryptoai-cryptowatch/
├── requirements.txt                      # Python dependencies
├── pyproject.toml                        # Project metadata
├── README.md                             # Project overview
├── .env.example                          # Environment variables template
├── .gitignore                            # Git ignore rules
│
├── cryptoai/                             # CryptoAI Trading System
│   ├── __init__.py
│   ├── agents/                           # Trading Agents
│   │   ├── __init__.py
│   │   ├── onchain_analyst.py            # On-chain analysis (LLM)
│   │   ├── social_sentiment.py           # Social sentiment (LLM)
│   │   ├── technical_analyst.py          # Technical analysis (LSTM-GNN)
│   │   ├── funding_analyst.py            # Funding rate analysis
│   │   └── manager_agent.py              # Signal fusion (LLM)
│   │
│   ├── data/                             # Data Ingestion
│   │   ├── __init__.py
│   │   ├── exchange_data.py              # Exchange APIs (ccxt)
│   │   ├── onchain_data.py               # On-chain data (web3.py)
│   │   ├── social_data.py                # Social media APIs
│   │   └── data_loader.py                # Historical data loader
│   │
│   ├── models/                           # ML Models
│   │   ├── __init__.py
│   │   ├── lstm_gnn.py                   # LSTM-GNN architecture
│   │   ├── garch_gru.py                  # GARCH-GRU volatility
│   │   ├── vwap_transformer.py           # VWAP execution
│   │   └── model_loader.py               # Model loading utilities
│   │
│   ├── risk/                             # Risk Management
│   │   ├── __init__.py
│   │   ├── volatility.py                 # Volatility forecasting
│   │   ├── var.py                        # VaR/CVaR calculation
│   │   ├── regime.py                     # Regime detection
│   │   └── circuit_breaker.py            # Circuit breakers
│   │
│   ├── portfolio/                        # Portfolio Construction
│   │   ├── __init__.py
│   │   ├── kelly.py                      # Kelly criterion
│   │   ├── position_sizing.py            # Position sizing
│   │   ├── diversification.py            # Diversification rules
│   │   └── rebalancing.py                # Rebalancing logic
│   │
│   ├── execution/                        # Trade Execution
│   │   ├── __init__.py
│   │   ├── vwap_execution.py             # VWAP slicing
│   │   ├── multi_exchange.py             # Multi-exchange routing
│   │   ├── slippage.py                   # Slippage optimization
│   │   └── broker_api.py                 # Broker integration
│   │
│   ├── backtesting/                      # Backtesting Engine
│   │   ├── __init__.py
│   │   ├── engine.py                     # Backtesting engine
│   │   ├── data_loader.py                # Historical data
│   │   ├── metrics.py                    # Performance metrics
│   │   └── walk_forward.py               # Walk-forward validation
│   │
│   ├── monitoring/                       # Monitoring
│   │   ├── __init__.py
│   │   ├── metrics.py                    # Prometheus metrics
│   │   ├── alerting.py                   # Alert system
│   │   └── dashboard.py                  # Grafana dashboards
│   │
│   ├── config/                           # Configuration
│   │   ├── __init__.py
│   │   ├── settings.py                   # Main settings
│   │   ├── agents.yaml                   # Agent configs
│   │   ├── risk.yaml                     # Risk configs
│   │   └── execution.yaml                # Execution configs
│   │
│   └── utils/                            # Utilities
│       ├── __init__.py
│       ├── logging.py                    # Logging setup
│       ├── helpers.py                    # Helper functions
│       └── constants.py                  # Constants
│
├── cryptowatch/                          # CryptoWatch Market Surveillance
│   ├── __init__.py
│   ├── ingestion/                        # Data Ingestion
│   │   ├── __init__.py
│   │   ├── exchange_ingestion.py         # Exchange data
│   │   ├── onchain_ingestion.py          # On-chain data
│   │   └── social_ingestion.py           # Social data
│   │
│   ├── detection/                        # Detection Engine
│   │   ├── __init__.py
│   │   ├── anomaly.py                    # Anomaly detection (ML)
│   │   ├── manipulation.py               # Manipulation detection
│   │   ├── whale.py                      # Whale detection
│   │   └── pump_group.py                 # Pump group detection
│   │
│   ├── alerts/                           # Alert System
│   │   ├── __init__.py
│   │   ├── telegram.py                   # Telegram bot
│   │   ├── slack.py                      # Slack integration
│   │   ├── sms.py                        # SMS (Twilio)
│   │   ├── email.py                      # Email (SendGrid)
│   │   └── webhook.py                    # Webhook for institutions
│   │
│   ├── dashboard/                        # Dashboard
│   │   ├── __init__.py
│   │   ├── streamlit_app.py              # MVP dashboard (Streamlit)
│   │   ├── components/                   # Dashboard components
│   │   └── static/                       # Static files
│   │
│   ├── compliance/                       # Compliance (Institutional)
│   │   ├── __init__.py
│   │   ├── reporting.py                  # Compliance reports
│   │   ├── audit_trail.py                # Audit trails
│   │   └── sla.py                        # SLA monitoring
│   │
│   ├── api/                              # API Layer
│   │   ├── __init__.py
│   │   ├── main.py                       # FastAPI app
│   │   ├── routes/                       # API routes
│   │   ├── websocket.py                  # WebSocket streaming
│   │   └── middleware.py                 # Middleware
│   │
│   ├── users/                            # User Management
│   │   ├── __init__.py
│   │   ├── auth.py                       # Authentication
│   │   ├── subscriptions.py              # Subscription management
│   │   └── preferences.py                # User preferences
│   │
│   └── billing/                          # Billing
│       ├── __init__.py
│       ├── stripe.py                     # Stripe integration
│       └── invoices.py                   # Invoice generation
│
├── shared/                               # Shared Infrastructure
│   ├── __init__.py
│   ├── database/                         # Database
│   │   ├── __init__.py
│   │   ├── connection.py                 # DB connection
│   │   ├── models.py                     # SQLAlchemy models
│   │   └── migrations/                   # Alembic migrations
│   │
│   ├── cache/                            # Cache Layer
│   │   ├── __init__.py
│   │   ├── redis_client.py               # Redis client
│   │   └── pubsub.py                     # Redis Pub/Sub
│   │
│   ├── logging/                          # Logging
│   │   ├── __init__.py
│   │   ├── setup.py                      # Logging setup
│   │   └── formatters.py                 # Log formatters
│   │
│   └── auth/                             # Authentication
│       ├── __init__.py
│       ├── jwt.py                        # JWT handling
│       └── api_keys.py                   # API key management
│
├── tests/                                # Testing
│   ├── __init__.py
│   ├── unit/                             # Unit Tests
│   │   ├── test_agents.py
│   │   ├── test_detection.py
│   │   └── test_risk.py
│   │
│   ├── integration/                      # Integration Tests
│   │   ├── test_data_pipeline.py
│   │   ├── test_alerts.py
│   │   └── test_api.py
│   │
│   └── e2e/                              # End-to-End Tests
│       ├── test_trading_flow.py
│       └── test_surveillance_flow.py
│
├── scripts/                              # Utility Scripts
│   ├── setup_database.sh                 # Database setup
│   ├── run_backtest.py                   # Run backtests
│   ├── optimize_params.py                # Hyperparameter optimization
│   ├── deploy.sh                         # Deployment script
│   └── monitor.sh                        # Monitoring script
│
├── docker/                               # Docker Configuration
│   ├── Dockerfile.api                    # API Dockerfile
│   ├── Dockerfile.worker                 # Worker Dockerfile
│   ├── Dockerfile.dashboard              # Dashboard Dockerfile
│   └── docker-compose.yml                # Docker Compose
│
├── deployment/                           # Deployment
│   ├── k8s/                              # Kubernetes
│   │   ├── namespace.yaml
│   │   ├── deployments/
│   │   ├── services/
│   │   └── configmaps/
│   │
│   └── terraform/                        # Terraform IaC
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
└── docs/                                 # Documentation
    ├── api/                              # API Documentation
    ├── architecture/                     # Architecture Docs
    ├── user-guides/                      # User Guides
    └── developer-guides/                 # Developer Guides
```

---

## Key Files to Create Next

### Priority 1 (Week 1)
1. `pyproject.toml` - Project metadata
2. `.env.example` - Environment template
3. `cryptoai/config/settings.py` - Main configuration
4. `shared/database/models.py` - Database schema
5. `docker/docker-compose.yml` - Local development

### Priority 2 (Week 2)
6. `cryptoai/agents/onchain_analyst.py` - First agent
7. `cryptoai/data/exchange_data.py` - Exchange ingestion
8. `cryptowatch/detection/whale.py` - Whale detection
9. `cryptowatch/alerts/telegram.py` - Telegram bot
10. `cryptowatch/dashboard/streamlit_app.py` - MVP dashboard

---

## Development Workflow

### Local Development
```bash
# 1. Clone repo
git clone https://github.com/rizoadev/cryptoai-cryptowatch.git
cd cryptoai-cryptowatch

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Setup environment
cp .env.example .env
# Edit .env with your API keys

# 5. Start services (Docker)
docker-compose up -d

# 6. Run migrations
alembic upgrade head

# 7. Start development
python -m cryptoai.agents.onchain_analyst
```

### Testing
```bash
# Run unit tests
pytest tests/unit

# Run integration tests
pytest tests/integration

# Run with coverage
pytest --cov=cryptoai --cov=cryptowatch tests/
```

### Deployment
```bash
# Build Docker images
docker-compose build

# Deploy to production
./scripts/deploy.sh production

# Monitor deployment
kubectl get pods -n cryptoai
```

---

## Next Steps

**Today (Week 1, Day 1):**
- [x] Generate `requirements.txt` ✅
- [x] Create directory structure ✅
- [ ] Create `pyproject.toml`
- [ ] Create `.env.example`
- [ ] Create `README.md`

**Tomorrow (Week 1, Day 2):**
- [ ] Setup PostgreSQL + TimescaleDB
- [ ] Setup Redis
- [ ] Create database models
- [ ] Run migrations

**Week 1 End:**
- [ ] All infrastructure setup
- [ ] Data ingestion working
- [ ] Ready to code agents

---

**© 2026 - Riza with Emmilia Help**  
*Built with courage, calculation, and AI collaboration*
