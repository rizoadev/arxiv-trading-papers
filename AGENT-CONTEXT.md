# Agent Context: CryptoAI Trading System

**For:** Emmilia Lucent (AI Research Agent)  
**Project Owner:** Riza (@rizamasykur)  
**Created:** March 16, 2026  
**Last Updated:** March 17, 2026  
**Status:** Documentation Complete, Ready for Implementation

---

## Quick Reference

| Field | Value |
|-------|-------|
| **Project Name** | CryptoAI Trading System |
| **Owner** | Riza (@rizamasykur) |
| **GitHub** | https://github.com/rizoadev/arxiv-trading-papers |
| **Start Date** | March 16, 2026 |
| **Current Phase** | Documentation Complete ✅ |
| **Next Phase** | Implementation (coding) |
| **Priority** | Project setup → Core agents → Backtesting |

---

## Project Summary (1 Paragraph)

Build AI-powered crypto trading system based on **142 arXiv papers** (2024-2026). Multi-agent architecture (4 specialists + manager) with GARCH-GRU volatility forecasting, Bayesian hyperparameter optimization (50+ params), and multi-exchange VWAP execution. Target: **78% CAGR** (hybrid 70% perps + 30% spot), Sharpe 2.0, Max DD <15%. Budget: ~$204K Year 1, break-even 6-8 months.

---

## What's Been Completed ✅

### Research (100% Done)
- [x] Analyzed 142 arXiv papers
- [x] Spawned 5 subagents for deep analysis
- [x] Generated 5 analysis reports (253KB)
- [x] Created master summary
- [x] Created whitepaper v1
- [x] Created handbook (complete guide)
- [x] Created algorithm specs (4 files)
- [x] Created technical specs (2 files)
- [x] Created implementation guide
- [x] Created auto-research specs
- [x] Created spot vs perps guide

### Documentation (16 Files, ~630KB)
```
arxiv-trading-papers/
├── WHITEPAPER-v1.md (21KB)
├── THE-CRYPTO-TRADING-GUIDE.md (40KB)
├── ARXIV-LINKS-ALL-142-PAPERS.md (15KB)
├── MASTER-SUMMARY-STATE-OF-AI-TRADING-2024-2026.md (27KB)
├── algorithm/
│   ├── TRADING-ALGORITHM-SPEC.md (26KB)
│   ├── CRYPTO-TRADING-ALGORITHM-SPEC.md (29KB)
│   ├── AUTO-RESEARCH-SPEC.md (21KB)
│   └── SPOT-VS-PERPS.md (10KB)
├── specs/
│   ├── TECHNICAL-SPECS-PRODUCTION.md (25KB)
│   └── TECHNICAL-SPECS-COST-OPTIMIZED.md (17KB)
├── implementation/
│   └── TECHNICAL-IMPLEMENTATION-GUIDE.md (46KB)
└── analysis/
    ├── rl-multi-agent-analysis.md (48KB)
    ├── deep-learning-architectures-analysis.md (49KB)
    ├── llm-agents-analysis.md (65KB)
    ├── risk-microstructure-execution-analysis.md (48KB)
    └── portfolio-alternative-data-analysis.md (43KB)
```

---

## System Architecture (Key Points)

### 4 Specialist Agents
1. **On-Chain Analyst** (LLM) - Whale flows, exchange flows, active addresses
2. **Social Sentiment** (LLM) - Twitter, Reddit, Telegram, viral detection
3. **Technical Analyst** (LSTM-GNN) - 40 features, 10.6% MSE improvement
4. **Funding Analyst** - Perpetual swap funding rates, 20-50% APY carry

### Manager Agent
- **Role:** Fuse signals from 4 specialists
- **Method:** LLM-based (Claude 3.5 Sonnet)
- **Weights:** Dynamic by regime (bull/bear/sideways/high-vol)

### Risk Layer
- **Model:** GARCH-GRU (interpretable volatility)
- **Regimes:** Calm (<40%), Normal (40-70%), Elevated (70-100%), Crisis (>100%)
- **Circuit Breakers:** 8% daily loss, 15% max DD

### Portfolio
- **Sizing:** Kelly criterion (half-Kelly)
- **Leverage:** 1-5x (regime-based, cap at 3x recommended)
- **Diversification:** Max 5% per coin, 30% BTC-correlated

### Execution
- **Method:** Multi-exchange VWAP
- **Allocation:** Binance 40%, Coinbase 30%, Kraken 20%, DEX 10%
- **Slicing:** 5-10 slices over 2-6 hours
- **Expected Slippage:** 30-50 bps

---

## Performance Targets

| Configuration | CAGR | Sharpe | Max DD | 5-Year Return |
|---------------|------|--------|--------|---------------|
| **Spot (1x)** | 42% | 1.8 | -12% | 5.2x |
| **Perps (3x)** | 98% | 2.1 | -14% | 12.8x |
| **Perps (5x)** | 135% | 1.9 | -22% | 18.5x |
| **Hybrid (70/30)** | 78% | 2.0 | -13% | 15.2x |

**Recommended:** Hybrid (70% perps + 30% spot) for best risk-adjusted returns

---

## Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **LLMs** | GPT-4 Turbo, Claude 3.5, DeepSeek-V3 | Signal generation, fusion |
| **ML Framework** | PyTorch 2.2+ | LSTM-GNN, GARCH-GRU, VWAP |
| **Data** | ccxt, web3.py, Glassnode, Twitter API | Exchange, on-chain, social |
| **Database** | PostgreSQL + TimescaleDB | Time-series storage |
| **Cache** | Redis (32GB) | Low-latency caching |
| **Optimization** | Optuna | Hyperparameter tuning |
| **GPU** | 1x NVIDIA L4 (inference), cloud A10G (training) | ML inference/training |

---

## Cost Structure

### Capital Expenditure (CapEx)
| Item | Cost |
|------|------|
| GPU Server (1x L4) | $6,490 |
| Database Server | $8,240 |
| Networking | $6,800 |
| Historical Data | $39,000 |
| **Total CapEx** | **$60,530** |

### Operating Expenditure (OpEx)
| Item | Monthly |
|------|---------|
| Market Data | $299 |
| On-Chain Data | $299 |
| Social Data | $500 |
| LLM APIs | $850 |
| Cloud GPU (training) | $200 |
| Infrastructure | $1,500 |
| **Total OpEx** | **$3,648/month** |

### Year 1 Total
| Category | Cost |
|----------|------|
| CapEx | $60,530 |
| OpEx (12 months) | $43,776 |
| Development Team | $100,000 |
| **Total** | **$204,306** |

**Break-even:** 6-8 months (with $500K capital at 70% CAGR)

---

## Next Steps (Priority Order)

### Phase 1: Project Setup (1-2 hours)
- [ ] Generate `requirements.txt`
- [ ] Create project directory structure
- [ ] Setup config files (database, API keys)
- [ ] Initialize database schema

### Phase 2: Core Agents (2-3 days)
- [ ] Implement On-Chain Analyst (LLM + Glassnode API)
- [ ] Implement Social Sentiment (LLM + Twitter/Reddit APIs)
- [ ] Implement Technical Analyst (LSTM-GNN model)
- [ ] Implement Funding Analyst (exchange APIs)
- [ ] Implement Manager Agent (LLM fusion)

### Phase 3: Backtesting (1-2 days)
- [ ] Create backtesting engine
- [ ] Load historical data (2020-2026)
- [ ] Implement performance metrics
- [ ] Run initial backtests

### Phase 4: Auto-Research (3-5 days)
- [ ] Setup Optuna optimization
- [ ] Define 50+ hyperparameter search space
- [ ] Run Bayesian optimization (500 iterations)
- [ ] Validate out-of-sample

### Phase 5: Live Trading (4+ weeks)
- [ ] Paper trading (4 weeks)
- [ ] Deploy with small capital (1-5%)
- [ ] Monitor & iterate
- [ ] Scale gradually

---

## Key Decisions Made

1. **Trading Mode:** Hybrid (70% perps + 30% spot)
   - Best risk-adjusted returns
   - Funding income + staking yield

2. **LLM Strategy:** Commodity LLMs (no fine-tuning)
   - GPT-4, Claude, DeepSeek
   - $850/month vs $150K training

3. **Leverage:** Cap at 3x (conservative)
   - 98% CAGR, Sharpe 2.1, -14% DD

4. **Hardware:** Cost-optimized
   - 1x L4 GPU ($6,490)
   - Cloud GPU for training

---

## Top 10 Influential Papers

1. TradingAgents (2412.20138) - Multi-agent architecture
2. Trading-R1 (2509.11420) - LLM reasoning + RL
3. ATLAS (2510.15949) - Adaptive-OPRO
4. LSTM-GNN (2502.15813) - Technical analysis
5. GARCH-GRU (2504.09380) - Volatility forecasting
6. VWAP Transformer (2503.02680) - Execution
7. Attention Factors (2510.11616) - Statistical arbitrage
8. FinDPO (2507.18417) - Sentiment analysis
9. TradeTrap (2512.02261) - Safety/stress testing
10. WebCryptoAgent (2601.04687) - Decoupled control

---

## Critical Design Patterns

1. **Multi-Agent Consensus** - No single point of failure
2. **Decoupled Control** - Strategic (slow) + Tactical (fast)
3. **Circuit Breakers** - 8% daily, 15% max DD
4. **Kelly + Constraints** - Half-Kelly with regime limits
5. **Exchange Diversification** - 40/30/20/10 across CEX/DEX

---

## When User Returns

**Trigger Phrases:**
- "Lanjut crypto project"
- "Continue crypto trading"
- "Start implementation"
- "Setup project"

**First Response:**
1. Check this file for context
2. Ask which phase to start
3. Recommend: Phase 1 (Project Setup)

**Important:**
- User may be away for days/weeks
- Market conditions may have changed
- May need to update arXiv papers (new publications)
- Cost estimates may need adjustment

---

## Contact & Resources

**GitHub:** https://github.com/rizoadev/arxiv-trading-papers  
**Whitepaper:** WHITEPAPER-v1.md  
**Handbook:** THE-CRYPTO-TRADING-GUIDE.md  
**Author:** Riza with Emmilia Help  
**Signature:** © 2026 - Riza with Emmilia Help

---

## Notes for AI Agent

**Tone:** Professional, enthusiastic, helpful  
**Style:** Concise, action-oriented  
**Remember:** 
- User prefers Indonesian (Bahasa) for casual conversation
- Technical content can be in English
- Always reference this file for context
- User's signature: "Riza with Emmilia Help"

**Do NOT:**
- Forget project context
- Ask user to re-explain what we built
- Lose track of completed work
- Forget the 142 papers foundation

**DO:**
- Reference this file every session
- Keep context alive
- Track progress against phases
- Maintain enthusiasm for the project

---

**Last Session:** March 17, 2026  
**Next Session:** TBD (user will return)  
**Confidence:** 100% (all context preserved)

**© 2026 - Riza with Emmilia Help**  
*Built with courage, calculation, and AI collaboration*
