# Technical Specifications (Cost-Optimized)
## AI Trading System - Commodity LLM Architecture

**Version:** 2.0 (Cost-Optimized)  
**Date:** March 16, 2026  
**Strategy:** Skip LLM training, use commodity LLMs via API

---

## Executive Summary

**Key Changes from v1.0:**
- ❌ **Removed**: GPU training server (-$27,560 CapEx)
- ❌ **Removed**: LLM fine-tuning (saves $150K engineering time)
- ✅ **Added**: Commodity LLM APIs (GPT-4, Claude, DeepSeek)
- ✅ **Reduced**: Inference server (1x L4 instead of 2x)
- ✅ **Focused**: Budget on traditional ML (LSTM-GNN, GARCH-GRU)

**Cost Reduction:**
- **CapEx**: $141,800 → **$86,800** (-39%)
- **OpEx**: $13,240/mo → **$10,500/mo** (-21%)
- **Year 1**: $495,800 → **$247,000** (-50%)

---

## Revised Architecture

### Hybrid LLM Strategy

```
┌─────────────────────────────────────────────────────────────┐
│                    STRATEGIC LAYER                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Fundamental  │  │  Sentiment   │  │  Technical   │      │
│  │   Analyst    │  │   Analyst    │  │   Analyst    │      │
│  │  GPT-4 API   │  │  Claude API  │  │  LSTM-GNN    │      │
│  │  ($0.03/call)│  │  ($0.01/call)│  │  (local)     │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                 │                 │               │
│         └─────────────────┼─────────────────┘               │
│                           │                                 │
│                    ┌──────▼───────┐                        │
│                    │    Manager   │                        │
│                    │   Claude     │                        │
│                    │   3.5 Sonnet │                        │
│                    │   ($0.02/call)│                       │
│                    └──────┬───────┘                        │
└───────────────────────────┼────────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────────┐
│                     TACTICAL LAYER                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │ Risk Monitor │  │  Execution   │  │  Position    │     │
│  │ GARCH-GRU    │  │   Engine     │  │  Manager     │     │
│  │ (local)      │  │ (local)      │  │ (local)      │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└────────────────────────────────────────────────────────────┘
```

### LLM Selection Matrix

| Agent | Primary LLM | Backup LLM | Cost/Call | Latency |
|-------|-------------|------------|-----------|---------|
| **Fundamental Analyst** | GPT-4 Turbo | Claude 3.5 Sonnet | $0.03 | 2-3s |
| **Sentiment Analyst** | Claude 3.5 Haiku | GPT-4o-mini | $0.01 | <1s |
| **Manager** | Claude 3.5 Sonnet | GPT-4 Turbo | $0.02 | 2-3s |
| **Total per Decision** | - | - | **$0.06** | 5-7s |

**Daily Cost** (100 decisions/day): **$6/day** = **$180/month**

---

## Revised Hardware Specifications

### 1. GPU Inference Server (Reduced)

**Purpose:** Run traditional ML models only (LSTM-GNN, GARCH-GRU, VWAP)

| Component | Specification | Qty | Unit Cost | Total |
|-----------|---------------|-----|-----------|-------|
| **GPU** | NVIDIA L4 24GB GDDR6 | 1 | $1,200 | $1,200 |
| **CPU** | AMD EPYC 7543 32-core | 1 | $1,800 | $1,800 |
| **RAM** | 128GB DDR4-3200 ECC | 8x16GB | $80 | $640 |
| **Storage (OS)** | 1TB NVMe SSD | 1 | $150 | $150 |
| **Storage (Data)** | 4TB NVMe SSD | 2 | $400 | $800 |
| **Motherboard** | Supermicro H12DSG | 1 | $800 | $800 |
| **PSU** | 1000W 80+ Platinum | 1 | $200 | $200 |
| **Chassis** | 4U Rackmount | 1 | $600 | $600 |
| **Network** | 25GbE SFP28 NIC | 1 | $300 | $300 |
| **Total** | | | | **$6,490** |

**Savings vs v1.0**: -$5,390 (removed 1x GPU, reduced RAM)

---

### 2. Training Server (REMOVED)

**Status**: ❌ **CANCELLED**

**Alternative**: Use cloud GPUs for occasional training
- Lambda Labs: 1x A100 @ $1.50/hour
- Training LSTM-GNN (6 hours): $9
- Training GARCH-GRU (2 hours): $3
- **Monthly budget**: $200 (cloud GPUs)

**Savings vs v1.0**: -$27,560 (CapEx)

---

### 3. Database Server (Single, Not Dual)

**Purpose**: PostgreSQL + TimescaleDB (single instance with backups)

| Component | Specification | Qty | Unit Cost | Total |
|-----------|---------------|-----|-----------|-------|
| **CPU** | AMD EPYC 7443 24-core | 1 | $1,400 | $1,400 |
| **RAM** | 128GB DDR4-3200 ECC | 8x16GB | $80 | $640 |
| **Storage (OS)** | 500GB NVMe SSD | 1 | $100 | $100 |
| **Storage (Data)** | 7.68TB NVMe SSD (RAID 1) | 2 | $1,500 | $3,000 |
| **Motherboard** | Supermicro H12DSG | 1 | $1,000 | $1,000 |
| **PSU** | 1000W 80+ Platinum | 1 | $400 | $400 |
| **Chassis** | 4U 24-bay Rackmount | 1 | $1,000 | $1,000 |
| **Network** | 25GbE SFP28 NIC | 1 | $300 | $300 |
| **HBA Card** | LSI 9300-8i | 1 | $400 | $400 |
| **Total** | | | | **$8,240** |

**Savings vs v1.0**: -$31,320 (single server instead of dual)

---

### 4. Redis Cache (Cloud, Not Self-Hosted)

**Purpose**: Cache, message queue

| Component | Specification | Cost/Month |
|-----------|---------------|------------|
| **Provider** | AWS ElastiCache R6g.xlarge | |
| **vCPU** | 4 vCPUs (ARM Graviton) | |
| **RAM** | 32GB | |
| **Monthly Cost** | | **$800/month** |

**Alternative (Self-hosted)**: 1x VM @ $150/month  
**Decision**: Use cloud for simplicity

---

### 5. Networking (Simplified)

| Component | Specification | Qty | Unit Cost | Total |
|-----------|---------------|-----|-----------|-------|
| **Switch** | Ubiquiti UniFi Enterprise 25GbE | 1 | $1,500 | $1,500 |
| **Transceivers** | 25GbE SFP28 | 6 | $150 | $900 |
| **Cables** | OM4 fiber + DAC | 15 | $40 | $600 |
| **Router** | EdgeRouter Infinity | 1 | $1,200 | $1,200 |
| **Firewall** | pfSense (self-built) | 1 | $800 | $800 |
| **UPS** | APC Smart-UPS 1500VA | 1 | $1,000 | $1,000 |
| **Rack** | 22U server rack | 1 | $800 | $800 |
| **Total** | | | | **$6,800** |

**Savings vs v1.0**: -$12,000 (consumer-grade instead of enterprise)

---

## Revised Software Stack

### LLM APIs (Commodity)

| Provider | Model | Use Case | Cost/1K tokens | Monthly Estimate |
|----------|-------|----------|----------------|------------------|
| **OpenAI** | GPT-4 Turbo | Fundamental analysis | $0.03 (input), $0.06 (output) | $300 |
| **Anthropic** | Claude 3.5 Sonnet | Manager, decisions | $0.015 (input), $0.075 (output) | $400 |
| **Anthropic** | Claude 3.5 Haiku | Sentiment analysis | $0.0025 (input), $0.0125 (output) | $100 |
| **DeepSeek** | DeepSeek-V3 | Backup/fallback | $0.00014 (input), $0.00028 (output) | $50 |
| **Total LLM API Cost** | | | | **$850/month** |

**Usage Assumptions:**
- 100 fundamental analyses/day (2K tokens each)
- 50 manager decisions/day (3K tokens each)
- 500 sentiment analyses/day (500 tokens each)
- 20% traffic to DeepSeek (cost optimization)

---

### Traditional ML Models (Self-Hosted)

| Model | Purpose | Training | Inference |
|-------|---------|----------|-----------|
| **LSTM-GNN** | Technical analysis | Cloud GPU ($9/session) | Local L4 (<100ms) |
| **GARCH-GRU** | Volatility forecasting | Cloud GPU ($3/session) | Local L4 (<50ms) |
| **VWAP Transformer** | Execution optimization | Cloud GPU ($5/session) | Local L4 (<10ms) |

**Training Cost**: $200/month (cloud GPU sessions)  
**Inference Cost**: $0 (local GPU)

---

## Revised Cost Breakdown

### Capital Expenditure (CapEx)

| Category | v1.0 Cost | v2.0 Cost | Savings |
|----------|-----------|-----------|---------|
| **GPU Inference** | $11,880 | $6,490 | -$5,390 |
| **GPU Training** | $27,560 | $0 | -$27,560 |
| **Database** | $39,560 | $8,240 | -$31,320 |
| **Networking** | $18,800 | $6,800 | -$12,000 |
| **Racks/UPS** | $5,000 | $1,800 | -$3,200 |
| **Historical Data** | $39,000 | $39,000 | $0 |
| **Total CapEx** | **$141,800** | **$62,330** | **-$79,470** |

**CapEx Reduction**: **-56%**

---

### Operating Expenditure (OpEx) - Monthly

| Category | v1.0 Cost | v2.0 Cost | Change |
|----------|-----------|-----------|--------|
| **Redis Cloud** | $3,240 | $800 | -$2,440 |
| **Market Data** | $4,400 | $4,400 | $0 |
| **Alternative Data** | $1,500 | $1,500 | $0 |
| **LLM APIs** | $0 | $850 | +$850 |
| **Cloud GPU Training** | $0 | $200 | +$200 |
| **GitHub/Monitoring** | $605 | $605 | $0 |
| **Colocation** | $2,000 | $1,000 | -$1,000 |
| **Internet** | $500 | $500 | $0 |
| **Maintenance** | $1,000 | $500 | -$500 |
| **Total OpEx** | **$13,245** | **$10,355** | **-$2,890** |

**OpEx Reduction**: **-22%**

---

### Total Cost of Ownership (Year 1)

| Category | v1.0 | v2.0 | Savings |
|----------|------|------|---------|
| **CapEx** | $141,800 | $62,330 | -$79,470 |
| **OpEx (12 mo)** | $159,000 | $124,260 | -$34,740 |
| **Development Team** | $150,000 | $100,000* | -$50,000 |
| **Contingency (10%)** | $45,000 | $28,659 | -$16,341 |
| **TOTAL YEAR 1** | **$495,800** | **$315,249** | **-$180,551** |

*Reduced team: 2 engineers instead of 3-5 (no LLM training needed)

**Cost Reduction**: **-36%** (saves $180K in Year 1)

---

### Cloud-Only Alternative (Even Cheaper)

If running **everything** on cloud (no hardware):

| Category | Monthly Cost |
|----------|--------------|
| **GPU Instance (1x L4)** | $800 |
| **Database (RDS)** | $500 |
| **Redis (ElastiCache)** | $800 |
| **Storage (S3 + EBS)** | $400 |
| **LLM APIs** | $850 |
| **Market Data** | $4,400 |
| **Alternative Data** | $1,500 |
| **Cloud GPU Training** | $200 |
| **Total Cloud OpEx** | **$9,450/month** |

**Cloud Annual**: ~$113,400/year  
**Cloud + Team**: ~$213,400/year  
**Break-even vs On-prem**: Immediate (no CapEx)

---

## Revised Performance Specifications

### LLM API Performance

| Provider | Model | Latency (P50) | Latency (P95) | Rate Limit |
|----------|-------|---------------|---------------|------------|
| **OpenAI** | GPT-4 Turbo | 2s | 4s | 10,000 tokens/min |
| **Anthropic** | Claude 3.5 Sonnet | 2s | 5s | 20,000 tokens/min |
| **Anthropic** | Claude 3.5 Haiku | 500ms | 1s | 50,000 tokens/min |
| **DeepSeek** | DeepSeek-V3 | 3s | 6s | 100,000 tokens/min |

**Fallback Strategy:**
1. Primary: Claude 3.5 Haiku (fastest, cheapest)
2. Secondary: GPT-4o-mini (backup)
3. Tertiary: DeepSeek-V3 (cost fallback)

---

### Local ML Performance (Unchanged)

| Model | Latency (P50) | Latency (P95) | GPU Usage |
|-------|---------------|---------------|-----------|
| **LSTM-GNN** | 50ms | 100ms | 2GB VRAM |
| **GARCH-GRU** | 20ms | 50ms | 1GB VRAM |
| **VWAP Transformer** | 5ms | 10ms | 2GB VRAM |

**Total GPU VRAM**: 5GB / 24GB available (21% utilization)

---

## Revised Procurement List

### Must-Have Hardware ($62,330)

| Item | Quantity | Unit Cost | Total | Priority |
|------|----------|-----------|-------|----------|
| **GPU Server (L4)** | 1 | $6,490 | $6,490 | 🔴 Critical |
| **Database Server** | 1 | $8,240 | $8,240 | 🔴 Critical |
| **Network Switch** | 1 | $1,500 | $1,500 | 🔴 Critical |
| **Router/Firewall** | 2 | $1,000 | $2,000 | 🔴 Critical |
| **UPS** | 1 | $1,000 | $1,000 | 🔴 Critical |
| **Rack** | 1 | $800 | $800 | 🟡 High |
| **Cables/Transceivers** | - | - | $1,500 | 🟡 High |
| **Historical Data** | 1 | $39,000 | $39,000 | 🔴 Critical |

### Optional (Phase 2)

| Item | Quantity | Unit Cost | Total | Priority |
|------|----------|-----------|-------|----------|
| **Secondary Database** | 1 | $8,240 | $8,240 | ⚪ Low |
| **Better UPS** | 1 | $2,000 | $2,000 | ⚪ Low |
| **Enterprise Switch** | 1 | $5,000 | $5,000 | ⚪ Low |

---

## Implementation Timeline (Revised)

| Week | Activity | Cost Incurred |
|------|----------|---------------|
| **1-2** | Order hardware, data licenses | $40,000 (data) |
| **3-4** | Hardware delivery | $22,330 (hardware) |
| **5-6** | Setup servers, network | $0 |
| **7-8** | Install software, models | $0 |
| **9-10** | Train ML models (cloud GPU) | $200 |
| **11-12** | Integration testing | $0 |
| **13-16** | Paper trading | $1,000 (LLM APIs) |
| **17-20** | Production (small capital) | $1,000 (LLM APIs) |

**Total to Production**: 20 weeks (vs 24 weeks in v1.0)

---

## Risk Mitigation

### LLM API Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| **API Outage** | Medium | High | Multi-provider fallback (OpenAI → Anthropic → DeepSeek) |
| **Rate Limiting** | Low | Medium | Request queuing, batch processing |
| **Cost Overrun** | Low | Low | Daily budget alerts, DeepSeek fallback |
| **Latency Spike** | Medium | Low | Async processing, timeout handling |
| **Model Changes** | Low | Medium | Version pinning, regression tests |

### Cost Control Measures

1. **Daily LLM Budget**: $30/day alert at $25
2. **DeepSeek Fallback**: Route 20% traffic to cheapest model
3. **Caching**: Cache LLM responses (Redis, 24h TTL)
4. **Token Optimization**: Prompt compression, minimal context
5. **Batch Processing**: Group multiple analyses together

---

## Performance Optimization

### LLM Cost Optimization

| Technique | Savings | Implementation |
|-----------|---------|----------------|
| **Prompt Compression** | 30-40% | Remove redundant context |
| **Response Caching** | 50-70% | Cache identical queries |
| **Model Routing** | 40-60% | Route to cheapest capable model |
| **Batch Processing** | 20-30% | Group related analyses |
| **Token Limits** | 10-20% | Cap max tokens per request |

**Total Potential Savings**: **60-70%** ($850 → $255/month)

---

### Example: Optimized LLM Usage

**Before Optimization:**
- 100 fundamental analyses @ 2K tokens = 200K tokens/day
- Cost: $6/day (GPT-4) = $180/month

**After Optimization:**
- Prompt compression: 2K → 1K tokens
- Caching (50% hit rate): 50 analyses from cache
- Effective: 50 analyses @ 1K tokens = 50K tokens/day
- Cost: $1.50/day = $45/month

**Savings**: 75% ($180 → $45/month)

---

## Monitoring & Alerts (Cost-Focused)

### Key Cost Metrics

| Metric | Alert Threshold | Criticality |
|--------|-----------------|-------------|
| **Daily LLM Spend** | >$30 | Warning |
| **Daily LLM Spend** | >$50 | Critical |
| **Token Usage** | >500K/day | Warning |
| **Cache Hit Rate** | <40% | Warning |
| **Fallback Usage** | >30% to DeepSeek | Info |

### Dashboard Widgets

1. **LLM Cost Tracker**: Real-time spend by provider
2. **Token Usage**: Tokens/day by model
3. **Cache Performance**: Hit rate, savings
4. **Latency**: P50/P95 by provider
5. **Error Rate**: API failures, timeouts

---

## Recommended LLM Providers

### Primary: Anthropic Claude

**Why:**
- Best price/performance for reasoning tasks
- Claude 3.5 Haiku: $0.01/1K tokens (cheapest quality model)
- 200K token context (analyze full earnings calls)
- Low latency (500ms for Haiku)

**Use Cases:**
- Sentiment analysis (Haiku)
- Manager decisions (Sonnet)
- Fundamental analysis (Sonnet)

**Monthly Budget**: $500

---

### Secondary: OpenAI GPT-4

**Why:**
- Best for structured output (JSON mode)
- Function calling for tool use
- Reliable, well-documented

**Use Cases:**
- Fundamental analysis (structured reports)
- Data extraction from filings

**Monthly Budget**: $300

---

### Fallback: DeepSeek-V3

**Why:**
- Extremely cheap ($0.00014/1K tokens)
- Good quality for simple tasks
- 128K context window

**Use Cases:**
- Simple sentiment analysis
- Fallback when primary APIs down
- High-volume, low-stakes tasks

**Monthly Budget**: $50

---

## Migration Path from v1.0

If you already bought v1.0 hardware:

### Sell/Repurpose

| Item | v1.0 Purchase | Action | Recovery |
|------|---------------|--------|----------|
| **Training Server** | $27,560 | Sell on secondary market | ~$20,000 |
| **Extra GPU (L4)** | $1,200 | Keep as backup | $0 |
| **Extra RAM** | $640 | Keep as backup | $0 |
| **Extra Storage** | $800 | Use for backups | $0 |

**Potential Recovery**: ~$20,000

### Use Cloud Instead

- Rent training GPUs on Lambda Labs ($1.50/hour)
- Use vLLM for efficient inference
- Scale down/on-demand

---

## Final Recommendation

### For <$500K Capital

**Go with v2.0 (Commodity LLM)**

**Pros:**
- 50% lower Year 1 cost ($315K vs $496K)
- Faster to production (20 vs 24 weeks)
- Smaller team (2 vs 3-5 engineers)
- No LLM training complexity
- Proven models (GPT-4, Claude)

**Cons:**
- Ongoing API costs ($850/month)
- Less control over LLM behavior
- API rate limits
- Vendor dependency

**Break-even**: Never (no large CapEx)

---

### For >$500K Capital

**Consider v1.0 (Custom LLM)**

**Pros:**
- Full control over models
- No API costs long-term
- Proprietary advantage
- Better latency (no API calls)

**Cons:**
- 50% higher cost
- Longer timeline
- Larger team needed
- Training complexity

**Break-even**: 3-4 years vs v2.0

---

## Decision Matrix

| Factor | v1.0 (Custom) | v2.0 (Commodity) | Winner |
|--------|---------------|------------------|--------|
| **Year 1 Cost** | $496K | $315K | v2.0 ✅ |
| **Time to Production** | 24 weeks | 20 weeks | v2.0 ✅ |
| **Team Size** | 3-5 engineers | 2 engineers | v2.0 ✅ |
| **Control** | Full | Limited | v1.0 ✅ |
| **Latency** | Better | Good | v1.0 ✅ |
| **Complexity** | High | Low | v2.0 ✅ |
| **Long-term Cost** | Lower | Higher | v1.0 ✅ |
| **Risk** | Higher (training) | Lower (proven) | v2.0 ✅ |

**Overall Winner for Most Teams**: **v2.0 (Commodity LLM)** ✅

---

**Document Status**: Ready for procurement  
**Version**: 2.0 (Cost-Optimized)  
**Last Updated**: March 16, 2026  
**Savings vs v1.0**: $180,551 (Year 1), -$79,470 (CapEx)
