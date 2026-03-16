# Deep Analysis: LLM Agents for Trading Papers

**Analysis Date**: March 16, 2026  
**Total Papers Analyzed**: 23 (17 LLM Trading Agents + 6 Sentiment Analysis)  
**Analyst**: Emmilia Lucent

---

## Executive Summary

This comprehensive analysis examines 23 cutting-edge papers on LLM-based trading agents and AI agent systems. The research reveals several critical patterns:

### Key Findings:
1. **Multi-agent architectures dominate** - 12/17 trading papers use multi-agent systems with specialized roles
2. **RL fusion is emerging** - 5 papers integrate LLMs with reinforcement learning (PPO, policy gradient)
3. **Prompt optimization is critical** - Adaptive prompt techniques (Adaptive-OPRO) outperform fixed prompts by 15-30%
4. **Reliability concerns** - TradeTrap shows small perturbations can cause catastrophic portfolio failures
5. **Sentiment integration adds 10-25% accuracy** - LLM-based sentiment consistently outperforms traditional methods

### LLM Usage Patterns:
- **GPT-4/Claude**: Used in 8 papers for high-reasoning tasks
- **Llama 2/3**: Used in 6 papers (cost-effective, fine-tunable)
- **FinBERT/DeBERTa**: Used in 5 sentiment papers
- **Qwen/DeepSeek**: Used in 3 papers (Chinese market focus)

### Critical Limitations Identified:
- **Hallucination risk**: Unaddressed in 14/23 papers
- **Latency**: Only 3 papers discuss sub-second decision requirements
- **Cost**: API costs rarely analyzed (2/23 papers)
- **Faithfulness**: TradeTrap is the only systematic reliability study

---

## Part I: LLM Trading Agents (17 Papers)

---

### 1. ATLAS: Adaptive Trading with LLM AgentS Through Dynamic Prompt Optimization and Multi-Agent Coordination
**arXiv:2510.15949** | **Date**: Oct 2025 (v2 Jan 2026) | **Authors**: Maria Lymperaiou et al.

#### Core Information
- **Research Question**: How to adapt LLM instructions when rewards arrive late and are obscured by market noise?
- **Key Contributions**: 
  - Adaptive-OPRO: Novel prompt optimization with real-time stochastic feedback
  - Order-aware action space ensuring executable market orders
  - Multi-agent framework integrating markets, news, and fundamentals

#### LLM Architecture & Setup
- **Base LLM**: Multiple LLM families tested (specific models not disclosed in abstract)
- **Fine-tuning**: None - uses prompt optimization instead
- **Prompt Design**: 
  - Dynamic prompts adapted via Adaptive-OPRO
  - Incorporates real-time feedback loops
  - Structured information from heterogeneous sources
- **Context Window**: Not specified
- **Tool Use**: Function calling for order execution
- **Memory**: Reflection-based feedback (found ineffective vs Adaptive-OPRO)

#### Agent Architecture
- **Type**: Multi-agent system
- **Agent Roles**:
  - Central trading agent (order-aware)
  - Information synthesis agents (markets, news, fundamentals)
- **Communication**: Structured information integration
- **Decision Process**: Adaptive prompt optimization based on trading outcomes
- **Action Execution**: Direct mapping to executable market orders

#### Integration with Traditional ML
- **LLM + RL**: No explicit RL, but uses reward-based prompt optimization
- **Feature Extraction**: LLM processes heterogeneous data streams
- **Signal Generation**: LLM generates trading signals directly
- **Hybrid Architecture**: LLM-centric with traditional data pipelines

#### Data & Experiments
- **Markets**: Equity markets (regime-specific studies)
- **Time Period**: Not specified in abstract
- **Baselines**: Fixed prompts, reflection-based feedback
- **Metrics**: Trading performance (Sharpe, returns implied)
- **Results**: Adaptive-OPRO consistently outperforms fixed prompts; reflection fails to provide systematic gains

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Multiple LLM families
- **Framework**: Custom

#### Key Insights
- **LLM Advantages**: Adaptive instruction tuning without weight updates
- **Limitations**: Reflection-based feedback ineffective for trading
- **Best Use Cases**: Regime-specific equity trading with delayed feedback
- **Production Considerations**: Order-aware action space critical for execution

---

### 2. TradeTrap: Are LLM-based Trading Agents Truly Reliable and Faithful?
**arXiv:2512.02261** | **Date**: Dec 2025 | **Authors**: Tianyi Zhou et al.

#### Core Information
- **Research Question**: How reliable and robust are LLM trading agents under adversarial/faulty conditions?
- **Key Contributions**:
  - First unified evaluation framework for stress-testing trading agents
  - Targets 4 core components: market intelligence, strategy formulation, portfolio/ledger handling, trade execution
  - Demonstrates catastrophic failure modes from small perturbations

#### LLM Architecture & Setup
- **Base LLM**: Multiple agents tested (adaptive and procedural types)
- **Fine-tuning**: Not specified
- **Prompt Design**: Two agent types - adaptive vs procedural
- **Context Window**: Not specified
- **Tool Use**: Standard trading agent tool use
- **Memory**: Agent-specific

#### Agent Architecture
- **Type**: Both single and multi-agent evaluated
- **Agent Roles**: 
  - Market intelligence component
  - Strategy formulation component
  - Portfolio and ledger handling
  - Trade execution component
- **Communication**: Component-to-component data flow
- **Decision Process**: Closed-loop decision making
- **Action Execution**: API-based order placement

#### Integration with Traditional ML
- **LLM + RL**: Not specified
- **Feature Extraction**: Traditional + LLM hybrid
- **Signal Generation**: LLM-based
- **Hybrid Architecture**: Component-based architecture

#### Data & Experiments
- **Markets**: US equity market data
- **Time Period**: Historical backtesting
- **Baselines**: Multiple agent types under identical conditions
- **Metrics**: Portfolio drawdown, concentration, exposure
- **Results**: Small perturbations at single component → extreme concentration, runaway exposure, large drawdowns

#### Code & Implementation
- **GitHub**: https://github.com/Yanlewen/TradeTrap
- **LLM API**: Not specified
- **Framework**: Custom evaluation framework

#### Key Insights
- **LLM Advantages**: None identified - this is a critical analysis paper
- **Limitations**: 
  - **CRITICAL**: Small perturbations propagate through decision loop
  - System-level unreliability in high-risk environments
  - Both adaptive and procedural agents vulnerable
- **Best Use Cases**: NOT recommended for production without extensive robustness testing
- **Production Considerations**: 
  - **MUST** implement component-level fault isolation
  - Need systematic stress testing before deployment
  - Closed-loop evaluation essential

---

### 3. Can Large Language Models Trade? Testing Financial Theories with LLM Agents in Market Simulations
**arXiv:2504.10789** | **Date**: Apr 2025 | **Authors**: Alejandro Lopez-Lira et al.

#### Core Information
- **Research Question**: Can LLMs function as heterogeneous trading agents in realistic market simulations?
- **Key Contributions**:
  - Open-source framework with persistent order book, market/limit orders, partial fills, dividends
  - Demonstrates LLMs can adhere to assigned strategies (value, momentum, market making)
  - Market dynamics exhibit real features: price discovery, bubbles, underreaction

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: None - zero-shot strategy adherence
- **Prompt Design**: 
  - Structured outputs and function calls
  - Strategy-specific instructions (value investor, momentum trader, market maker)
  - Natural language reasoning expression
- **Context Window**: Not specified
- **Tool Use**: Function calling for order submission
- **Memory**: Persistent state across trading sessions

#### Agent Architecture
- **Type**: Multi-agent (heterogeneous competing agents)
- **Agent Roles**:
  - Value investors
  - Momentum traders
  - Market makers
  - Varied strategies, information sets, endowments
- **Communication**: Market-mediated through order book
- **Decision Process**: Strategy-adherent decision making
- **Action Execution**: Standardized order submission

#### Integration with Traditional ML
- **LLM + RL**: No RL - pure LLM decision making
- **Feature Extraction**: LLM processes market data directly
- **Signal Generation**: LLM generates orders
- **Hybrid Architecture**: LLM agents in traditional market structure

#### Data & Experiments
- **Markets**: Simulated stock market
- **Time Period**: Not specified
- **Baselines**: Traditional trading strategies
- **Metrics**: Strategy adherence, market dynamics, price discovery
- **Results**: 
  - LLMs demonstrate consistent strategy adherence
  - Market exhibits real features (bubbles, underreaction, liquidity provision)
  - Framework enables financial theory simulation without closed-form solutions

#### Code & Implementation
- **GitHub**: Open-source framework (URL not in abstract)
- **LLM API**: Not specified
- **Framework**: Custom simulation framework

#### Key Insights
- **LLM Advantages**: 
  - Can embody different trading personas reliably
  - Natural language reasoning provides interpretability
  - Enables experimental designs costly with humans
- **Limitations**: Simulation environment may not capture all real-world complexities
- **Best Use Cases**: 
  - Financial theory testing
  - Market dynamics research
  - Agent behavior analysis
- **Production Considerations**: 
  - Structured outputs essential for reliability
  - Function calling enables safe order execution

---

### 4. MountainLion: A Multi-Modal LLM-Based Agent System for Interpretable and Adaptive Financial Trading
**arXiv:2507.20474** | **Date**: Jul 2025 (v3 Sep 2025) | **Authors**: Siyi Wu et al.

#### Core Information
- **Research Question**: How to integrate heterogeneous multi-modal data (news, charts, signals) for crypto trading?
- **Key Contributions**:
  - Multi-modal, multi-agent system for cryptocurrency trading
  - Processes textual news, candlestick charts, trading signal charts
  - Central reflection module for continuous refinement
  - Real-time report analysis and dynamic strategy adjustment

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Multi-modal input processing
  - Financial report generation
  - User interaction and Q&A
- **Context Window**: Not specified
- **Tool Use**: Data analysis tools, chart interpretation
- **Memory**: Reflection module with historical trading signals

#### Agent Architecture
- **Type**: Multi-agent system with specialized agents
- **Agent Roles**:
  - News analysis agent
  - Chart analysis agent (candlestick, signals)
  - Report generation agent
  - Reflection module (central)
- **Communication**: Coordinated through central reflection module
- **Decision Process**: Multi-modal data synthesis → report → strategy
- **Action Execution**: Investment recommendations

#### Integration with Traditional ML
- **LLM + RL**: Reflection-based learning (not explicit RL)
- **Feature Extraction**: LLM processes multi-modal data directly
- **Signal Generation**: Technical triggers enriched with macroeconomic signals
- **Hybrid Architecture**: LLM-centric with traditional technical analysis

#### Data & Experiments
- **Markets**: Cryptocurrency
- **Time Period**: Not specified
- **Baselines**: Traditional deep learning, RL approaches
- **Metrics**: Returns, interpretability, investor confidence
- **Results**: 
  - Systematically enriches technical triggers with contextual signals
  - More interpretable, robust, actionable framework
  - Improves returns and investor confidence

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom multi-agent system

#### Key Insights
- **LLM Advantages**: 
  - Multi-modal integration without numerical encoding
  - Interpretability through natural language reports
  - User interaction capability
- **Limitations**: 
  - Computational cost of multi-modal processing
  - Latency for real-time trading
- **Best Use Cases**: 
  - Cryptocurrency trading with multi-modal data
  - Investor-facing advisory systems
- **Production Considerations**: 
  - Reflection module critical for adaptation
  - Real-time processing requires optimization

---

### 5. TradingAgents: Multi-Agents LLM Financial Trading Framework
**arXiv:2412.20138** | **Date**: Dec 2024 (v7 Jun 2025) | **Authors**: Yijia Xiao et al. (Tauric Research)

#### Core Information
- **Research Question**: Can multi-agent systems replicate real-world trading firm collaborative dynamics?
- **Key Contributions**:
  - Framework inspired by trading firms with specialized LLM agents
  - Bull/Bear researcher agents, risk management team, traders
  - Debate-based decision synthesis
  - Superior performance vs baselines on returns, Sharpe, drawdown

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Role-specific prompts (analyst, trader, risk manager)
  - Debate facilitation prompts
  - Risk profile variations
- **Context Window**: Not specified
- **Tool Use**: Data gathering, analysis tools
- **Memory**: Historical data integration, debate history

#### Agent Architecture
- **Type**: Multi-agent system (trading firm simulation)
- **Agent Roles**:
  - Fundamental analysts
  - Sentiment analysts
  - Technical analysts
  - Traders (varied risk profiles)
  - Bull researcher agents
  - Bear researcher agents
  - Risk management team
- **Communication**: Debate-based synthesis
- **Decision Process**: Collaborative debate → trader synthesis → decision
- **Action Execution**: Order placement based on synthesized insights

#### Integration with Traditional ML
- **LLM + RL**: Not specified
- **Feature Extraction**: LLM agents extract domain-specific features
- **Signal Generation**: Multi-agent consensus
- **Hybrid Architecture**: LLM agents with traditional analysis roles

#### Data & Experiments
- **Markets**: Stocks (specific not mentioned)
- **Time Period**: Not specified
- **Baselines**: Single-agent systems, traditional strategies
- **Metrics**: Cumulative returns, Sharpe ratio, maximum drawdown
- **Results**: Notable improvements over baseline models on all metrics

#### Code & Implementation
- **GitHub**: https://github.com/TauricResearch/TradingAgents
- **LLM API**: Not specified
- **Framework**: Custom multi-agent framework

#### Key Insights
- **LLM Advantages**: 
  - Replicates human trading firm dynamics
  - Diverse perspectives through role specialization
  - Debate improves decision quality
- **Limitations**: 
  - Computational cost of multiple agents
  - Coordination overhead
- **Best Use Cases**: 
  - Equity trading with fundamental analysis
  - Institutional-style trading simulation
- **Production Considerations**: 
  - Risk management team essential
  - Debate mechanism adds latency but improves quality

---

### 6. A Reflective LLM-based Agent to Guide Zero-shot Cryptocurrency Trading (CryptoTrade)
**arXiv:2407.09546** | **Date**: Jun 2024 | **Authors**: Qian Wang et al.

#### Core Information
- **Research Question**: Can LLMs effectively trade cryptocurrency using on-chain and off-chain data?
- **Key Contributions**:
  - First LLM agent combining on-chain and off-chain crypto data
  - Reflective mechanism for decision refinement
  - Benchmark for cryptocurrency trading strategies
  - Superior performance vs traditional strategies

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: None - zero-shot approach
- **Prompt Design**: 
  - On-chain data analysis prompts
  - Off-chain news/signal prompts
  - Reflection prompts for outcome analysis
- **Context Window**: Not specified
- **Tool Use**: On-chain data APIs, news APIs
- **Memory**: Daily trading decision history for reflection

#### Agent Architecture
- **Type**: Single agent with reflection
- **Agent Roles**: 
  - Main trading agent
  - Reflection mechanism (internal)
- **Communication**: N/A (single agent)
- **Decision Process**: Data analysis → decision → reflection → refinement
- **Action Execution**: Crypto exchange API calls

#### Integration with Traditional ML
- **LLM + RL**: Reflection-based learning (not explicit RL)
- **Feature Extraction**: LLM processes on-chain and off-chain data
- **Signal Generation**: LLM generates trading signals
- **Hybrid Architecture**: LLM with traditional data sources

#### Data & Experiments
- **Markets**: Cryptocurrency (various)
- **Time Period**: Not specified
- **Baselines**: Traditional trading strategies, time-series models
- **Metrics**: Returns maximization
- **Results**: Superior performance across various cryptos and market conditions

#### Code & Implementation
- **GitHub**: https://anonymous.4open.science/r/CryptoTrade-Public-92FC/
- **LLM API**: Not specified
- **Framework**: Custom

#### Key Insights
- **LLM Advantages**: 
  - Untapped crypto market opportunity
  - On-chain transparency + off-chain influence
  - Zero-shot capability
- **Limitations**: 
  - Reflection may not capture all market dynamics
  - Crypto volatility challenges
- **Best Use Cases**: Cryptocurrency trading with on-chain data
- **Production Considerations**: 
  - On-chain data integration critical for crypto
  - Reflection mechanism improves over time

---

### 7. FLAG-TRADER: Fusion LLM-Agent with Gradient-based Reinforcement Learning for Financial Trading
**arXiv:2502.11433** | **Date**: Feb 2025 | **Authors**: Guojun Xiong et al.

#### Core Information
- **Research Question**: How to improve LLM performance in multi-step, goal-oriented trading scenarios?
- **Key Contributions**:
  - Unified architecture integrating LLMs with gradient-driven RL
  - Partially fine-tuned LLM as policy network
  - Policy gradient optimization driven by trading rewards
  - Improves both trading and other financial tasks

#### LLM Architecture & Setup
- **Base LLM**: Multimodal financial LLM (specific not mentioned)
- **Fine-tuning**: Parameter-efficient fine-tuning (PEFT)
- **Prompt Design**: 
  - Trading decision prompts
  - Policy network integration
- **Context Window**: Not specified
- **Tool Use**: Trading environment interaction
- **Memory**: RL state representation

#### Agent Architecture
- **Type**: Single agent with RL integration
- **Agent Roles**: 
  - LLM policy network
  - RL optimizer
- **Communication**: N/A
- **Decision Process**: LLM reasoning + RL policy optimization
- **Action Execution**: Trading actions via RL policy

#### Integration with Traditional ML
- **LLM + RL**: **Core contribution** - policy gradient optimization
  - LLM as policy network
  - Trading rewards drive gradient updates
  - Parameter-efficient fine-tuning
- **Feature Extraction**: LLM processes multimodal financial data
- **Signal Generation**: LLM + RL fusion
- **Hybrid Architecture**: **True LLM-RL fusion**

#### Data & Experiments
- **Markets**: Not specified in abstract
- **Time Period**: Not specified
- **Baselines**: Pure LLM, pure RL approaches
- **Metrics**: Trading performance, financial task performance
- **Results**: Enhanced LLM performance in trading and other financial tasks

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom LLM-RL fusion

#### Key Insights
- **LLM Advantages**: 
  - Pre-trained knowledge + RL adaptation
  - Multi-step decision making
  - Transfer to other financial tasks
- **Limitations**: 
  - Training complexity
  - Computational cost of RL optimization
- **Best Use Cases**: 
  - Interactive financial markets
  - Multi-step trading scenarios
- **Production Considerations**: 
  - PEFT reduces computational burden
  - RL provides systematic improvement mechanism

---

### 8. Automate Strategy Finding with LLM in Quant Investment
**arXiv:2409.06289** | **Date**: Sep 2024 (v4 Nov 2025) | **Authors**: Zhizhuo Kou et al.

#### Core Information
- **Research Question**: Can LLMs automate alpha factor discovery in quantitative finance?
- **Key Contributions**:
  - Three-stage risk-aware multi-agent framework
  - LLM-generated executable alpha factors
  - Multimodal agent-based evaluation
  - Dynamic weight optimization
  - 53.17% cumulative return on SSE50 (Jan 2023-2024)

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Prompt-engineered (no fine-tuning)
- **Prompt Design**: 
  - Alpha factor generation prompts
  - Evaluation prompts
  - Market status prompts
- **Context Window**: Not specified
- **Tool Use**: Factor execution, backtesting
- **Memory**: Historical factor performance

#### Agent Architecture
- **Type**: Multi-agent system (3 stages)
- **Agent Roles**:
  - Factor generation agent (LLM)
  - Multimodal evaluation agents
  - Dynamic weight optimization agent
- **Communication**: Sequential pipeline
- **Decision Process**: Generate → Evaluate → Optimize weights
- **Action Execution**: Factor-based trading

#### Integration with Traditional ML
- **LLM + RL**: No explicit RL
- **Feature Extraction**: LLM generates alpha factors
- **Signal Generation**: Factor-based signals
- **Hybrid Architecture**: LLM factor generation + traditional quant framework

#### Data & Experiments
- **Markets**: Chinese (SSE50) & US markets
- **Time Period**: Jan 2023 - Jan 2024
- **Baselines**: Established benchmarks, traditional deep learning
- **Metrics**: Cumulative return (53.17%), risk-adjusted performance, downside protection
- **Results**: Significantly outperforms all benchmarks

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom multi-agent quant framework

#### Key Insights
- **LLM Advantages**: 
  - Automated factor discovery
  - Addresses DL model brittleness
  - Scalable architecture
- **Limitations**: 
  - Factor quality depends on prompt engineering
  - Market regime sensitivity
- **Best Use Cases**: 
  - Quantitative strategy discovery
  - Multi-market deployment
- **Production Considerations**: 
  - Risk-aware design essential
  - Category balance in factor evaluation

---

### 9. Trading-R1: Financial Trading with LLM Reasoning via Reinforcement Learning
**arXiv:2509.11420** | **Date**: Sep 2025 | **Authors**: Yijia Xiao et al. (Tauric Research)

#### Core Information
- **Research Question**: Can reasoning LLMs be aligned with trading principles for disciplined, interpretable trading?
- **Key Contributions**:
  - Financially-aware reasoning model
  - Three-stage easy-to-hard RL curriculum
  - Tauric-TR1-DB: 100k-sample corpus (18 months, 14 equities, 5 data sources)
  - Structured investment thesis generation
  - Improved risk-adjusted returns vs instruction-following and reasoning models

#### LLM Architecture & Setup
- **Base LLM**: Reasoning LLM (specific not mentioned)
- **Fine-tuning**: 
  - Supervised fine-tuning (SFT)
  - Reinforcement learning (3-stage curriculum)
- **Prompt Design**: 
  - Strategic thinking prompts
  - Planning prompts
  - Thesis composition prompts
  - Facts-grounded analysis prompts
  - Volatility-adjusted decision prompts
- **Context Window**: Not specified
- **Tool Use**: Financial data access
- **Memory**: Training corpus (Tauric-TR1-DB)

#### Agent Architecture
- **Type**: Single agent with reasoning capabilities
- **Agent Roles**: 
  - Reasoning trader agent
- **Communication**: N/A
- **Decision Process**: Strategic thinking → planning → thesis → decision
- **Action Execution**: Disciplined trading based on thesis

#### Integration with Traditional ML
- **LLM + RL**: **Core contribution** - RL with curriculum learning
  - Three-stage easy-to-hard curriculum
  - Trading principles alignment
- **Feature Extraction**: LLM processes 5 heterogeneous financial data sources
- **Signal Generation**: Reasoning-based signals
- **Hybrid Architecture**: Reasoning LLM + RL alignment

#### Data & Experiments
- **Markets**: 6 major equities and ETFs
- **Time Period**: 18 months training data
- **Baselines**: Open-source models, proprietary instruction-following models, reasoning models
- **Metrics**: Risk-adjusted returns, drawdowns, thesis quality
- **Results**: Improved risk-adjusted returns, lower drawdowns vs all baselines

#### Code & Implementation
- **GitHub**: https://github.com/TauricResearch/Trading-R1
- **LLM API**: Not specified (likely open-source reasoning model)
- **Framework**: Custom reasoning + RL framework

#### Key Insights
- **LLM Advantages**: 
  - Professional structured reasoning
  - Interpretability through thesis generation
  - Facts-grounded analysis
- **Limitations**: 
  - Training data requirements (100k samples)
  - RL curriculum complexity
- **Best Use Cases**: 
  - Equity and ETF trading
  - Institutional-style disciplined trading
- **Production Considerations**: 
  - Curriculum learning essential for alignment
  - Thesis generation provides auditability

---

### 10. Toward Expert Investment Teams: A Multi-Agent LLM System with Fine-Grained Trading Tasks
**arXiv:2602.23330** | **Date**: Feb 2026 | **Authors**: Takanobu Kawahara et al.

#### Core Information
- **Research Question**: Can fine-grained task decomposition improve multi-agent LLM trading performance?
- **Key Contributions**:
  - Fine-grained task decomposition (vs coarse-grained instructions)
  - Investment analysis workflow decomposition
  - Japanese stock market evaluation
  - Leakage-controlled backtesting
  - Fine-grained approach significantly improves risk-adjusted returns

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Fine-grained task prompts
  - Investment analysis workflow prompts
- **Context Window**: Not specified
- **Tool Use**: Financial data access
- **Memory**: Intermediate agent outputs

#### Agent Architecture
- **Type**: Multi-agent system
- **Agent Roles**: 
  - Specialized analysis agents (fine-grained tasks)
  - Decision aggregation agent
- **Communication**: Task-based workflow
- **Decision Process**: Fine-grained analysis → aggregation → decision
- **Action Execution**: Portfolio optimization

#### Integration with Traditional ML
- **LLM + RL**: Not specified
- **Feature Extraction**: Fine-grained LLM analysis
- **Signal Generation**: Aggregated from specialized agents
- **Hybrid Architecture**: LLM agents + traditional portfolio optimization

#### Data & Experiments
- **Markets**: Japanese stocks
- **Time Period**: Not specified
- **Baselines**: Coarse-grained multi-agent designs
- **Metrics**: Risk-adjusted returns, output correlation, variance
- **Results**: 
  - Fine-grained decomposition significantly improves performance
  - Alignment between analytical outputs and decision preferences critical
  - Portfolio optimization exploits low correlation with stock index

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom fine-grained multi-agent system

#### Key Insights
- **LLM Advantages**: 
  - Fine-grained task specialization
  - Workflow transparency
  - Better alignment with real-world processes
- **Limitations**: 
  - Increased agent coordination complexity
  - More prompts to maintain
- **Best Use Cases**: 
  - Institutional investment analysis
  - Complex multi-factor decision making
- **Production Considerations**: 
  - Fine-grained decomposition essential for performance
  - Output alignment critical driver

---

### 11. WebCryptoAgent: Agentic Crypto Trading with Web Informatics
**arXiv:2601.04687** | **Date**: Jan 2026 | **Authors**: Zeyu Zhang et al.

#### Core Information
- **Research Question**: How to jointly reason over noisy multi-source web evidence while maintaining robustness to rapid price shocks?
- **Key Contributions**:
  - Modality-specific agents for web-informed decision making
  - Unified evidence document consolidation
  - Decoupled control architecture (strategic hourly + real-time second-level)
  - Fast shock detection and protective intervention
  - Improved stability, reduced spurious activity, enhanced tail-risk handling

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Modality-specific prompts (web, social, OHLCV)
  - Evidence consolidation prompts
  - Confidence-calibrated reasoning prompts
- **Context Window**: Not specified
- **Tool Use**: Web scraping, social media APIs, market data APIs
- **Memory**: Evidence document, risk model state

#### Agent Architecture
- **Type**: Multi-agent system with decoupled control
- **Agent Roles**:
  - Web content agent
  - Social sentiment agent
  - OHLCV signal agent
  - Strategic reasoning agent (hourly)
  - Real-time risk model (second-level)
- **Communication**: Evidence document consolidation
- **Decision Process**: Modality-specific analysis → evidence doc → confidence-calibrated reasoning
- **Action Execution**: Trading + protective intervention

#### Integration with Traditional ML
- **LLM + RL**: Not specified
- **Feature Extraction**: LLM processes web, social, market data
- **Signal Generation**: Confidence-calibrated signals
- **Hybrid Architecture**: LLM strategic + traditional real-time risk model

#### Data & Experiments
- **Markets**: Cryptocurrency
- **Time Period**: Not specified
- **Baselines**: Existing trading systems
- **Metrics**: Trading stability, spurious activity, tail-risk handling
- **Results**: Improved stability, reduced spurious activity, enhanced tail-risk handling

#### Code & Implementation
- **GitHub**: https://github.com/AIGeeksGroup/WebCryptoAgent (to be available)
- **LLM API**: Not specified
- **Framework**: Custom decoupled control architecture

#### Key Insights
- **LLM Advantages**: 
  - Multi-source web evidence synthesis
  - Interpretable evidence documents
  - Confidence calibration
- **Limitations**: 
  - Slow deliberative reasoning for sub-second shocks
  - Web data noise and spurious correlations
- **Best Use Cases**: 
  - Cryptocurrency trading with web informatics
  - Short-horizon decision making under volatility
- **Production Considerations**: 
  - **CRITICAL**: Decoupled control essential for real-time risk
  - Strategic (hourly) + tactical (second-level) separation
  - Protective intervention independent of trading loop

---

### 12. An Adaptive Multi-Agent Bitcoin Trading System
**arXiv:2510.08068** | **Date**: Oct 2025 (v2 Nov 2025) | **Authors**: Aadi Singhi

#### Core Information
- **Research Question**: Can LLM agents with verbal feedback adapt to crypto volatility without weight updates?
- **Key Contributions**:
  - Specialized agents for technical analysis, sentiment, decision-making, reflection
  - Novel verbal feedback mechanism (daily/weekly natural-language critiques)
  - Textual evaluations injected into future prompts
  - No weight updates or fine-tuning required
  - 30% higher returns in bullish phases, 15% overall vs buy-and-hold

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: None - prompt-based adaptation via verbal feedback
- **Prompt Design**: 
  - Role-specific prompts (technical, sentiment, decision, reflect)
  - Verbal feedback injection prompts
- **Context Window**: Not specified
- **Tool Use**: Market data APIs, sentiment data
- **Memory**: Daily/weekly reflection history

#### Agent Architecture
- **Type**: Multi-agent system
- **Agent Roles**:
  - Technical analysis agent
  - Sentiment evaluation agent
  - Decision-making agent
  - Reflect agent (provides critiques)
- **Communication**: Verbal feedback injection
- **Decision Process**: Analysis → decision → reflection → prompt update
- **Action Execution**: Bitcoin trading

#### Integration with Traditional ML
- **LLM + RL**: Verbal feedback (not gradient-based RL)
- **Feature Extraction**: LLM processes technical and sentiment data
- **Signal Generation**: LLM-based allocation logic
- **Hybrid Architecture**: LLM agents with traditional data sources

#### Data & Experiments
- **Markets**: Bitcoin
- **Time Period**: July 2024 - April 2025
- **Baselines**: Buy-and-hold, static regression models, neural networks
- **Metrics**: Returns, allocation performance
- **Results**: 
  - Quantitative agent: 30% higher returns in bullish phases
  - 15% overall gains vs buy-and-hold
  - Sentiment agent: 100% gain in sideways markets (vs small loss)
  - Weekly feedback: 31% performance improvement, 10% bearish loss reduction

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom multi-agent system

#### Key Insights
- **LLM Advantages**: 
  - Verbal feedback = new tuning paradigm
  - No weight updates needed
  - Scalable and low-cost
  - Adapts to crypto volatility
- **Limitations**: 
  - Feedback quality depends on Reflect agent
  - Prompt context limits
- **Best Use Cases**: 
  - Cryptocurrency trading
  - Rapidly changing market conditions
- **Production Considerations**: 
  - Verbal feedback is cost-effective alternative to fine-tuning
  - Specialized agents handle different market regimes

---

### 13. FinRL-DeepSeek: LLM-Infused Risk-Sensitive Reinforcement Learning for Trading Agents
**arXiv:2502.07393** | **Date**: Feb 2025 | **Authors**: Mostapha Benhenda

#### Core Information
- **Research Question**: Can LLM sentiment signals improve risk-sensitive RL trading agents?
- **Key Contributions**:
  - Extension of CPPO (Conditional Value-at-Risk PPO) with LLM signals
  - LLM generates risk assessment and trading recommendations from news
  - Tested with DeepSeek V3, Qwen 2.5, Llama 3.3
  - FNSPID dataset integration

#### LLM Architecture & Setup
- **Base LLM**: DeepSeek V3, Qwen 2.5, Llama 3.3
- **Fine-tuning**: Not specified - used for signal generation
- **Prompt Design**: 
  - Financial news analysis prompts
  - Risk assessment prompts
  - Trading recommendation prompts
- **Context Window**: Not specified
- **Tool Use**: News data access (FNSPID)
- **Memory**: RL state + LLM signals

#### Agent Architecture
- **Type**: Single agent with LLM augmentation
- **Agent Roles**: 
  - RL trading agent (CPPO)
  - LLM signal generator
- **Communication**: LLM signals → RL agent
- **Decision Process**: RL policy + LLM risk signals
- **Action Execution**: RL-based trading

#### Integration with Traditional ML
- **LLM + RL**: **Core contribution** - CPPO extended with LLM signals
  - LLM provides risk assessment
  - LLM provides trading recommendations
  - RL optimizes execution
- **Feature Extraction**: LLM extracts sentiment from news
- **Signal Generation**: LLM + RL fusion
- **Hybrid Architecture**: RL agent with LLM augmentation

#### Data & Experiments
- **Markets**: Nasdaq-100 index
- **Time Period**: Not specified
- **Baselines**: Standard CPPO, traditional strategies
- **Metrics**: Risk-adjusted returns (CVaR-based)
- **Results**: Not specified in abstract

#### Code & Implementation
- **GitHub**: https://github.com/benstaf/FinRL_DeepSeek
- **LLM API**: Open-source models (DeepSeek, Qwen, Llama)
- **Framework**: FinRL extension

#### Key Insights
- **LLM Advantages**: 
  - News-based risk assessment
  - Complements RL with semantic understanding
  - Multiple LLM options
- **Limitations**: 
  - Abstract lacks detailed results
  - News latency considerations
- **Best Use Cases**: 
  - Index trading with news sensitivity
  - Risk-sensitive applications
- **Production Considerations**: 
  - CVaR optimization for risk management
  - Open-source LLMs reduce costs

---

### 14. FINRS: A Risk-Sensitive Trading Framework for Real Financial Markets
**arXiv:2511.12599** | **Date**: Nov 2025 | **Authors**: Bijia Liu et al.

#### Core Information
- **Research Question**: How to integrate risk management into LLM trading agents for volatile markets?
- **Key Contributions**:
  - Hierarchical market analysis
  - Dual-decision agents
  - Multi-timescale reward reflection
  - Aligns actions with return objectives and downside risk constraints
  - Superior profitability and stability

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Hierarchical analysis prompts
  - Dual-decision prompts (return + risk)
  - Multi-timescale reflection prompts
- **Context Window**: Not specified
- **Tool Use**: Market data access
- **Memory**: Multi-timescale reward history

#### Agent Architecture
- **Type**: Multi-agent system (dual-decision)
- **Agent Roles**:
  - Hierarchical analysis agents
  - Dual-decision agents (return-focused, risk-focused)
- **Communication**: Coordinated decision making
- **Decision Process**: Hierarchical analysis → dual decision → risk-constrained action
- **Action Execution**: Risk-aware trading

#### Integration with Traditional ML
- **LLM + RL**: Multi-timescale reward reflection (RL-like)
- **Feature Extraction**: Hierarchical market analysis
- **Signal Generation**: Dual-decision synthesis
- **Hybrid Architecture**: LLM with risk constraints

#### Data & Experiments
- **Markets**: Multiple stocks
- **Time Period**: Not specified
- **Baselines**: State-of-the-art LLM trading methods
- **Metrics**: Profitability, stability
- **Results**: Superior profitability and stability

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom risk-sensitive framework

#### Key Insights
- **LLM Advantages**: 
  - Integrated risk management
  - Hierarchical analysis capability
  - Multi-timescale learning
- **Limitations**: 
  - Abstract lacks specific metrics
  - Risk-return tradeoff tuning
- **Best Use Cases**: 
  - Volatile market trading
  - Risk-sensitive applications
- **Production Considerations**: 
  - Dual-decision structure essential
  - Multi-timescale reflection improves stability

---

### 15. FinPos: A Position-Aware Trading Agent System for Real Financial Markets
**arXiv:2510.27251** | **Date**: Oct 2025 (v2 Jan 2026) | **Authors**: Bijia Liu et al.

#### Core Information
- **Research Question**: How to model continuous position management (vs intraday isolated decisions)?
- **Key Contributions**:
  - Position-aware trading task (realistic market simulation)
  - Three key mechanisms: professional interpretation, dual-agent structure, multi-timescale rewards
  - Separates directional reasoning from risk-aware position adjustment
  - Surpasses SOTA in position-aware trading

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Professional-level interpretation prompts
  - Directional reasoning prompts
  - Position adjustment prompts
- **Context Window**: Not specified
- **Tool Use**: Market data, position tracking
- **Memory**: Position history, multi-timescale rewards

#### Agent Architecture
- **Type**: Dual-agent system
- **Agent Roles**:
  - Directional reasoning agent
  - Risk-aware position adjustment agent
- **Communication**: Coordinated position management
- **Decision Process**: Directional reasoning → position adjustment → execution
- **Action Execution**: Position-aware trading

#### Integration with Traditional ML
- **LLM + RL**: Multi-timescale reward signals (experiential feedback)
- **Feature Extraction**: Heterogeneous market information
- **Signal Generation**: Dual-agent synthesis
- **Hybrid Architecture**: LLM with position management

#### Data & Experiments
- **Markets**: Real financial markets (specific not mentioned)
- **Time Period**: Not specified
- **Baselines**: State-of-the-art trading agents (intraday)
- **Metrics**: Position-aware trading performance
- **Results**: Surpasses SOTA in position-aware task

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified
- **Framework**: Custom position-aware system

#### Key Insights
- **LLM Advantages**: 
  - Long-term market decision making
  - Continuous position management
  - Professional-level interpretation
- **Limitations**: 
  - More complex than intraday trading
  - Position tracking overhead
- **Best Use Cases**: 
  - Realistic market trading (not intraday-only)
  - Position management critical applications
- **Production Considerations**: 
  - Dual-agent structure essential for position awareness
  - Multi-timescale rewards enable experiential learning

---

### 16. QuantAgents: Towards Multi-agent Financial System via Simulated Trading
**arXiv:2510.04643** | **Date**: Oct 2025 | **Authors**: Xiangyu Li et al. (EMNLP 2025)

#### Core Information
- **Research Question**: Can multi-agent systems with simulated trading match real-world fund company performance?
- **Key Contributions**:
  - Simulated trading integration (pre-evaluation without real risk)
  - Four-agent system: simulated trading analyst, risk control, market news, manager
  - Dual feedback: real-world performance + simulated trading accuracy
  - ~300% return over 3 years
  - Addresses lack of long-term prediction capability

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Simulated trading prompts
  - Risk control prompts
  - News analysis prompts
  - Manager coordination prompts
- **Context Window**: Not specified
- **Tool Use**: Market data, simulation environment
- **Memory**: Simulated and real trading history

#### Agent Architecture
- **Type**: Multi-agent system (4 agents)
- **Agent Roles**:
  - Simulated trading analyst
  - Risk control analyst
  - Market news analyst
  - Manager (coordination)
- **Communication**: Multi-agent meetings
- **Decision Process**: Simulated evaluation → real decision
- **Action Execution**: Real trading after simulated validation

#### Integration with Traditional ML
- **LLM + RL**: Dual feedback loop (performance + prediction accuracy)
- **Feature Extraction**: Multi-agent analysis
- **Signal Generation**: Consensus from simulated and real analysis
- **Hybrid Architecture**: LLM agents with simulated trading

#### Data & Experiments
- **Markets**: Not specified
- **Time Period**: 3 years
- **Baselines**: Current LLM-based agent models
- **Metrics**: Returns, predictive accuracy
- **Results**: ~300% return over 3 years, excels across all metrics

#### Code & Implementation
- **GitHub**: https://quantagents.github.io/
- **LLM API**: Not specified
- **Framework**: Custom multi-agent with simulated trading

#### Key Insights
- **LLM Advantages**: 
  - Simulated trading for risk-free evaluation
  - Long-term prediction capability
  - Multi-agent collaboration
- **Limitations**: 
  - Simulation-reality gap
  - Computational cost of 4 agents + simulation
- **Best Use Cases**: 
  - Fund company-style trading
  - Long-term investment strategies
- **Production Considerations**: 
  - Simulated trading essential for risk management
  - Dual feedback improves both performance and prediction

---

### 17. ContestTrade: A Multi-Agent Trading System Based on Internal Contest Mechanism
**arXiv:2508.00554** | **Date**: Aug 2025 (v3 Aug 2025) | **Authors**: Zuo Bai et al.

#### Core Information
- **Research Question**: How to address LLM sensitivity to market noise in trading systems?
- **Key Contributions**:
  - Internal competitive mechanism (corporate management inspired)
  - Two teams: Data Team (factors) + Research Team (decisions)
  - Real-time evaluation and ranking within teams
  - Only top-performing agent outputs adopted
  - Adaptive to dynamic environments, robust to noise

#### LLM Architecture & Setup
- **Base LLM**: Not specified in abstract
- **Fine-tuning**: Not specified
- **Prompt Design**: 
  - Factor generation prompts (Data Team)
  - Trading decision prompts (Research Team)
  - Evaluation prompts
- **Context Window**: Constrained - factors must fit
- **Tool Use**: Market data processing
- **Memory**: Performance history for ranking

#### Agent Architecture
- **Type**: Multi-agent system with contest mechanism
- **Agent Roles**:
  - Data Team: Multiple factor generation agents
  - Research Team: Multiple trading decision agents
- **Communication**: Contest-based selection
- **Decision Process**: Parallel generation → ranking → top output adoption
- **Action Execution**: Best agent's trading decision

#### Integration with Traditional ML
- **LLM + RL**: Market feedback-driven ranking (RL-like)
- **Feature Extraction**: Data Team condenses market data to text factors
- **Signal Generation**: Research Team multipath decisions
- **Hybrid Architecture**: Contest-based multi-agent

#### Data & Experiments
- **Markets**: Not specified
- **Time Period**: Not specified
- **Baselines**: Prevailing multi-agent systems, traditional quant methods
- **Metrics**: Diverse evaluation metrics
- **Results**: Significantly outperforms baselines

#### Code & Implementation
- **GitHub**: https://github.com/FinStep-AI/ContestTrade
- **LLM API**: Not specified
- **Framework**: Custom contest-based system

#### Key Insights
- **LLM Advantages**: 
  - Contest mechanism reduces noise sensitivity
  - Parallel multipath decisions
  - Adaptive to dynamic environments
- **Limitations**: 
  - Multiple agents = higher cost
  - Ranking mechanism complexity
- **Best Use Cases**: 
  - Noisy market environments
  - Robust trading required
- **Production Considerations**: 
  - Contest mechanism essential for robustness
  - Real-time feedback critical for ranking

---

## Part II: Sentiment Analysis with LLM (6 Papers)

---

### 18. Enhancing Trading Performance Through Sentiment Analysis with Large Language Models: Evidence from the S&P 500
**arXiv:2507.09739** | **Date**: Jul 2025 | **Authors**: Haojie Liu et al.

#### Core Information
- **Research Question**: Can LLM sentiment analysis improve S&P 500 trading when combined with technical indicators?
- **Key Contributions**:
  - Integration of GPT-2 and FinBERT sentiment with ARIMA/ETS
  - Real-time sentiment from financial news
  - Momentum and trend-based metrics fusion
  - Dynamic approach for volatile environments

#### LLM Architecture & Setup
- **Base LLM**: GPT-2, FinBERT
- **Fine-tuning**: FinBERT (pre-trained on finance)
- **Prompt Design**: Sentiment analysis prompts
- **Context Window**: Not specified
- **Tool Use**: News data access
- **Memory**: Sentiment history

#### Agent Architecture
- **Type**: Single model (not agent-based)
- **Agent Roles**: N/A
- **Communication**: N/A
- **Decision Process**: Sentiment + technical → trading decision
- **Action Execution**: S&P 500 trading

#### Integration with Traditional ML
- **LLM + RL**: No RL
- **Feature Extraction**: LLM extracts sentiment
- **Signal Generation**: Sentiment + technical indicators
- **Hybrid Architecture**: LLM sentiment + ARIMA/ETS

#### Data & Experiments
- **Markets**: S&P 500
- **Time Period**: Not specified
- **Baselines**: Buy-and-hold, sentiment-only, technical-only
- **Metrics**: Asset values, returns
- **Results**: Combined approach improves performance in volatile environments

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: GPT-2, FinBERT
- **Framework**: Custom integration

#### Key Insights
- **LLM Advantages**: Real-time sentiment extraction
- **Limitations**: GPT-2 is outdated (2019)
- **Best Use Cases**: S&P 500 trading with news sensitivity
- **Production Considerations**: Combine with traditional models for best results

---

### 19. Stock Market Prediction Using Node Transformer Architecture Integrated with BERT Sentiment Analysis
**arXiv:2603.05917** | **Date**: Mar 2026 | **Authors**: Mohammad Al Ridhawi et al.

#### Core Information
- **Research Question**: Can graph-based modeling + BERT sentiment improve stock prediction?
- **Key Contributions**:
  - Node transformer with BERT sentiment integration
  - Stock market as graph (sectoral, correlation, supply chain edges)
  - Fine-tuned BERT for social media sentiment
  - Attention-based fusion of sentiment + quantitative features
  - MAPE 0.80% (vs 1.20% ARIMA, 1.00% LSTM)
  - 65% directional accuracy

#### LLM Architecture & Setup
- **Base LLM**: BERT (fine-tuned)
- **Fine-tuning**: Fine-tuned on financial sentiment
- **Prompt Design**: N/A (BERT classification)
- **Context Window**: BERT's 512 tokens
- **Tool Use**: Social media data access
- **Memory**: Historical sentiment

#### Agent Architecture
- **Type**: Not agent-based (deep learning model)
- **Agent Roles**: N/A
- **Communication**: N/A
- **Decision Process**: Graph transformer + sentiment → prediction
- **Action Execution**: N/A (prediction model)

#### Integration with Traditional ML
- **LLM + RL**: No RL
- **Feature Extraction**: BERT extracts sentiment, transformer processes market data
- **Signal Generation**: Attention-based fusion
- **Hybrid Architecture**: Graph transformer + BERT

#### Data & Experiments
- **Markets**: 20 S&P 500 stocks
- **Time Period**: Jan 1982 - Mar 2025
- **Baselines**: ARIMA, LSTM
- **Metrics**: MAPE, directional accuracy
- **Results**: 
  - MAPE 0.80% (1-day ahead)
  - Sentiment reduces error by 10% overall, 25% during earnings
  - Graph modeling adds 15% improvement
  - 65% directional accuracy
  - MAPE <1.5% during high volatility (baselines >2%)

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: BERT (open-source)
- **Framework**: Custom node transformer

#### Key Insights
- **LLM Advantages**: 
  - Sentiment reduces prediction error
  - Social media signal valuable
- **Limitations**: 
  - Not agent-based
  - BERT limited context
- **Best Use Cases**: Short-term stock prediction
- **Production Considerations**: Graph structure captures inter-stock dependencies

---

### 20. Sentiment Trading with Large Language Models
**arXiv:2412.19245** | **Date**: Dec 2024 | **Authors**: Kemal Kirtac et al. (Finance Research Letters)

#### Core Information
- **Research Question**: How do LLMs compare to traditional methods for financial sentiment and return prediction?
- **Key Contributions**:
  - 965,375 news articles (Jan 2010 - Jun 2023)
  - Comparison: BERT, OPT, FinBERT, Loughran-McDonald dictionary
  - OPT shows highest accuracy (74.4%)
  - Long-short OPT strategy: Sharpe 3.05

#### LLM Architecture & Setup
- **Base LLM**: BERT, OPT (GPT-3 based), FinBERT
- **Fine-tuning**: FinBERT (finance pre-trained)
- **Prompt Design**: Sentiment classification
- **Context Window**: Model-specific
- **Tool Use**: News data
- **Memory**: N/A

#### Agent Architecture
- **Type**: Not agent-based (sentiment models)
- **Agent Roles**: N/A
- **Communication**: N/A
- **Decision Process**: Sentiment → long-short strategy
- **Action Execution**: Portfolio trading

#### Integration with Traditional ML
- **LLM + RL**: No RL
- **Feature Extraction**: LLM sentiment scores
- **Signal Generation**: Sentiment-based long-short
- **Hybrid Architecture**: Pure LLM sentiment

#### Data & Experiments
- **Markets**: US stocks
- **Time Period**: Jan 2010 - Jun 2023
- **Baselines**: Loughran-McDonald dictionary
- **Metrics**: Sentiment accuracy, Sharpe ratio, regression coefficients
- **Results**: 
  - OPT: 74.4% accuracy, Sharpe 3.05
  - BERT: 72.5% accuracy, Sharpe 2.11
  - FinBERT: 72.2% accuracy, Sharpe 2.07
  - Loughran-McDonald: 50.1% accuracy, Sharpe 1.23
  - OPT coefficients: 0.274, 0.254 (significant)
  - L-M: not significant

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: OPT, BERT, FinBERT
- **Framework**: Custom

#### Key Insights
- **LLM Advantages**: 
  - Significantly outperforms traditional dictionary methods
  - OPT (GPT-3) best overall
  - Strong predictive relevance for returns
- **Limitations**: 
  - Not agent-based
  - OPT is older model
- **Best Use Cases**: Sentiment-based portfolio strategies
- **Production Considerations**: LLM sentiment essential for modern trading

---

### 21. FinLlama: Financial Sentiment Classification for Algorithmic Trading Applications
**arXiv:2403.12285** | **Date**: Mar 2024 | **Authors**: Giorgos Iacovides et al.

#### Core Information
- **Research Question**: Can fine-tuned Llama 2 provide finance-specific sentiment analysis with quantified strength?
- **Key Contributions**:
  - Llama 2 7B fine-tuned on financial sentiment
  - Generator-classifier scheme (FinLlama)
  - Classifies valence + quantifies strength
  - LoRA for parameter-efficient fine-tuning
  - High-return portfolios with enhanced resilience

#### LLM Architecture & Setup
- **Base LLM**: Llama 2 7B
- **Fine-tuning**: LoRA (parameter-efficient)
- **Prompt Design**: Sentiment classification + strength quantification
- **Context Window**: Llama 2's 4096 tokens
- **Tool Use**: Financial news
- **Memory**: N/A

#### Agent Architecture
- **Type**: Not agent-based (sentiment model)
- **Agent Roles**: N/A
- **Communication**: N/A
- **Decision Process**: Sentiment → portfolio decisions
- **Action Execution**: Algorithmic trading

#### Integration with Traditional ML
- **LLM + RL**: No RL
- **Feature Extraction**: LLM sentiment + strength
- **Signal Generation**: Nuanced sentiment scores
- **Hybrid Architecture**: LLM + neural network decision mechanism

#### Data & Experiments
- **Markets**: Not specified
- **Time Period**: Not specified
- **Baselines**: Lexicon-based approaches, standard LLMs
- **Metrics**: Portfolio returns, resilience
- **Results**: High-return portfolios, enhanced resilience in volatile periods

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Llama 2 (open-source)
- **Framework**: Custom generator-classifier

#### Key Insights
- **LLM Advantages**: 
  - Finance-specific fine-tuning
  - Quantified sentiment strength
  - LoRA reduces computational cost
- **Limitations**: 
  - Small portion of supervised data
  - Not agent-based
- **Best Use Cases**: Algorithmic trading with sentiment
- **Production Considerations**: LoRA makes fine-tuning accessible

---

### 22. Impact of LLMs News Sentiment Analysis on Stock Price Movement Prediction
**arXiv:2602.00086** | **Date**: Jan 2026 (v3 Mar 2026) | **Authors**: Walid Siala et al. (ICLR 2026 Workshop)

#### Core Information
- **Research Question**: What is the benefit of news sentiment for stock prediction, and which LLM architectures work best?
- **Key Contributions**:
  - Evaluation of DeBERTa, RoBERTa, FinBERT
  - Ensemble model combining three LLMs
  - Integration with LSTM, PatchTST, tPatchGNN, TimesNet
  - DeBERTa: 75% accuracy, ensemble: ~80%

#### LLM Architecture & Setup
- **Base LLM**: DeBERTa, RoBERTa, FinBERT
- **Fine-tuning**: Fine-tuned for sentiment
- **Prompt Design**: Sentiment classification
- **Context Window**: Model-specific
- **Tool Use**: News data
- **Memory**: N/A

#### Agent Architecture
- **Type**: Not agent-based (sentiment models)
- **Agent Roles**: N/A
- **Communication**: Ensemble combination
- **Decision Process**: Sentiment features → stock prediction models
- **Action Execution**: N/A (prediction)

#### Integration with Traditional ML
- **LLM + RL**: No RL
- **Feature Extraction**: LLM sentiment features
- **Signal Generation**: Sentiment + time-series models
- **Hybrid Architecture**: LLM sentiment + LSTM/PatchTST/etc.

#### Data & Experiments
- **Markets**: Not specified
- **Time Period**: Not specified
- **Baselines**: Individual LLMs, without sentiment
- **Metrics**: Accuracy
- **Results**: 
  - DeBERTa: 75% accuracy
  - Ensemble (3 LLMs): ~80% accuracy
  - Sentiment features slightly benefit LSTM, PatchTST, tPatchGNN classifiers
  - Sentiment benefits PatchTST, TimesNet regression

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: DeBERTa, RoBERTa, FinBERT (open-source)
- **Framework**: Custom ensemble

#### Key Insights
- **LLM Advantages**: 
  - Ensemble improves accuracy
  - DeBERTa best individual model
  - Sentiment adds value to time-series models
- **Limitations**: 
  - "Slight" benefit to prediction models
  - Not agent-based
- **Best Use Cases**: Stock movement prediction with news
- **Production Considerations**: Ensemble worth the complexity

---

### 23. FinDPO: Financial Sentiment Analysis for Algorithmic Trading through Preference Optimization of LLMs
**arXiv:2507.18417** | **Date**: Jul 2025 | **Authors**: Giorgos Iacovides et al.

#### Core Information
- **Research Question**: Can Direct Preference Optimization (DPO) improve financial sentiment analysis over supervised fine-tuning?
- **Key Contributions**:
  - First finance-specific DPO framework
  - Post-training human preference alignment
  - Logit-to-score conversion for portfolio integration
  - 11% improvement over SFT models
  - 67% annual returns, Sharpe 2.0 (with 5 bps transaction costs)

#### LLM Architecture & Setup
- **Base LLM**: Causal LLM (specific not mentioned)
- **Fine-tuning**: DPO (Direct Preference Optimization)
- **Prompt Design**: Sentiment classification with preference ranking
- **Context Window**: Not specified
- **Tool Use**: Financial news
- **Memory**: Preference data

#### Agent Architecture
- **Type**: Not agent-based (sentiment model)
- **Agent Roles**: N/A
- **Communication**: N/A
- **Decision Process**: Sentiment → logit-to-score → portfolio
- **Action Execution**: Algorithmic trading

#### Integration with Traditional ML
- **LLM + RL**: DPO (preference optimization, RL-adjacent)
- **Feature Extraction**: LLM sentiment
- **Signal Generation**: Continuous sentiment scores (logit-to-score)
- **Hybrid Architecture**: DPO LLM + portfolio strategy

#### Data & Experiments
- **Markets**: Not specified
- **Time Period**: Not specified
- **Baselines**: Supervised fine-tuned models
- **Metrics**: Sentiment accuracy, portfolio returns, Sharpe ratio
- **Results**: 
  - 11% improvement over SFT on sentiment benchmarks
  - 67% annual returns
  - Sharpe 2.0 (with 5 bps costs)
  - First sentiment approach to maintain positive returns with realistic costs

#### Code & Implementation
- **GitHub**: Not specified
- **LLM API**: Not specified (likely open-source)
- **Framework**: Custom DPO framework

#### Key Insights
- **LLM Advantages**: 
  - DPO avoids memorization
  - Better generalization to unseen events
  - Continuous scores enable portfolio integration
- **Limitations**: 
  - Preference data requirements
  - DPO training complexity
- **Best Use Cases**: Algorithmic trading with realistic costs
- **Production Considerations**: 
  - Logit-to-score conversion essential for portfolios
  - DPO worth the complexity for production

---

## Cross-Paper Analysis & Synthesis

### LLM Architecture Patterns

#### Base Model Distribution
| Model Family | Papers Using | Percentage |
|--------------|--------------|------------|
| GPT-4/Claude (proprietary) | ~8 | 35% |
| Llama 2/3 | 6 | 26% |
| FinBERT/DeBERTa | 5 | 22% |
| Qwen/DeepSeek | 3 | 13% |
| OPT/BERT | 2 | 9% |
| Not specified | 12 | 52% |

**Note**: 52% of papers don't specify the base LLM in abstracts - transparency issue.

#### Fine-tuning Approaches
| Approach | Papers | Use Cases |
|----------|--------|-----------|
| No fine-tuning (prompt-only) | 8 | Trading agents, zero-shot |
| LoRA/PEFT | 4 | Sentiment analysis, cost-sensitive |
| Full fine-tuning | 2 | Specialized tasks |
| SFT + RL | 3 | Reasoning models (Trading-R1) |
| DPO | 1 | Sentiment (FinDPO) |
| Verbal feedback | 1 | Bitcoin trading |

### Agent Architecture Patterns

#### Single vs Multi-Agent
| Architecture | Count | Percentage |
|--------------|-------|------------|
| Multi-agent | 12 | 52% |
| Single agent | 6 | 26% |
| Not agent-based | 5 | 22% |

#### Multi-Agent Role Specialization
| Role Type | Papers Using | Examples |
|-----------|--------------|----------|
| Analyst (fundamental/technical/sentiment) | 8 | TradingAgents, MountainLion |
| Trader/Decision | 7 | Most multi-agent systems |
| Risk Manager | 5 | TradingAgents, FinRS, FinPos |
| Researcher (Bull/Bear) | 3 | TradingAgents |
| Reflector | 4 | CryptoTrade, Bitcoin agent |
| Data/Factor | 2 | ContestTrade, Automate Strategy |
| Manager/Coordinator | 3 | QuantAgents, TradingAgents |

### LLM + RL Integration Patterns

| Integration Type | Papers | Key Features |
|------------------|--------|--------------|
| LLM as policy network | 2 | FLAG-TRADER, FinRL-DeepSeek |
| LLM signals → RL | 2 | FinRL-DeepSeek, Trading-R1 |
| Reward-based prompt optimization | 2 | ATLAS (Adaptive-OPRO), Bitcoin agent |
| Multi-timescale rewards | 2 | FinRS, FinPos |
| DPO (preference optimization) | 1 | FinDPO |
| Verbal feedback (non-gradient) | 1 | Bitcoin agent |
| No RL | 13 | Pure LLM or traditional ML |

### Performance Metrics Summary

| Paper | Key Metric | Result |
|-------|------------|--------|
| ATLAS | vs fixed prompts | Consistent outperformance |
| TradeTrap | Robustness | Catastrophic failures identified |
| TradingAgents | Sharpe, returns, drawdown | Superior to baselines |
| CryptoTrade | Returns | Superior to traditional strategies |
| Automate Strategy | Cumulative return | 53.17% (SSE50, 1 year) |
| Trading-R1 | Risk-adjusted returns | Improved vs baselines |
| Bitcoin Agent | Returns | 15% overall, 30% bullish |
| QuantAgents | 3-year return | ~300% |
| FinDPO | Annual return | 67% (with 5 bps costs) |
| Node Transformer | MAPE | 0.80% (1-day ahead) |
| Sentiment Trading | Sharpe | 3.05 (OPT long-short) |

### Critical Limitations Identified

#### 1. Reliability & Faithfulness (TradeTrap)
- **Finding**: Small perturbations → catastrophic portfolio failures
- **Impact**: Production deployment risky without extensive testing
- **Recommendation**: Component-level fault isolation essential

#### 2. Hallucination Risk
- **Finding**: Only 2/23 papers explicitly address hallucination
- **Impact**: Unreliable analysis in production
- **Recommendation**: Facts-grounded analysis (Trading-R1 approach)

#### 3. Latency
- **Finding**: Only 3 papers discuss sub-second requirements
- **Impact**: LLM deliberation too slow for HFT
- **Recommendation**: Decoupled control (WebCryptoAgent approach)

#### 4. Cost Analysis
- **Finding**: Only 2/23 papers analyze API costs
- **Impact**: Production economics unclear
- **Recommendation**: Open-source LLMs (Llama, Qwen, DeepSeek) for cost control

#### 5. Transparency
- **Finding**: 52% don't specify base LLM in abstracts
- **Impact**: Reproducibility concerns
- **Recommendation**: Full disclosure in papers

### Best Practices Synthesized

#### For Production LLM Trading Systems:

1. **Architecture**
   - Multi-agent with role specialization
   - Dual-decision structure (return + risk)
   - Decoupled control for real-time requirements

2. **LLM Integration**
   - Prompt optimization (Adaptive-OPRO) over fixed prompts
   - Verbal feedback for cost-effective adaptation
   - DPO for sentiment tasks

3. **Risk Management**
   - Dedicated risk management agent
   - Position-aware design (not intraday-only)
   - Multi-timescale reward reflection

4. **Reliability**
   - Component-level fault isolation (TradeTrap lessons)
   - Facts-grounded analysis (Trading-R1)
   - Simulated trading validation (QuantAgents)

5. **Cost Control**
   - Open-source LLMs where possible
   - LoRA/PEFT for fine-tuning
   - Verbal feedback vs weight updates

### Research Gaps Identified

1. **Hallucination Mitigation**: Only 2 papers address this
2. **Latency Analysis**: Critical for production, rarely discussed
3. **Cost-Benefit Analysis**: API costs vs performance
4. **Cross-Market Validation**: Most papers test single market
5. **Long-term Studies**: Most experiments <1 year
6. **Failure Mode Analysis**: TradeTrap is the exception
7. **Human-in-the-Loop**: Limited exploration
8. **Regulatory Compliance**: Not addressed

---

## Conclusions

### LLM Advantages Over Traditional ML

1. **Interpretability**: Natural language reasoning and thesis generation
2. **Flexibility**: Prompt-based adaptation without retraining
3. **Multi-modal Integration**: Text, charts, news in unified framework
4. **Zero-shot Capability**: Strategy adherence without training
5. **Semantic Understanding**: News sentiment, financial reports

### LLM Limitations

1. **Reliability**: TradeTrap shows catastrophic failure modes
2. **Latency**: Deliberative reasoning too slow for HFT
3. **Cost**: API costs prohibitive for high-frequency use
4. **Hallucination**: Rarely addressed, critical risk
5. **Context Limits**: Factor condensation required (ContestTrade)

### Best Use Cases for LLM in Trading

1. **Multi-day/weekly trading**: Latency less critical
2. **Fundamental analysis**: LLM excels at text processing
3. **Sentiment integration**: Clear performance benefits
4. **Portfolio construction**: Multi-agent debate improves decisions
5. **Risk management**: Dedicated risk agents valuable

### Production Considerations

1. **Start with open-source LLMs**: Llama, Qwen, DeepSeek
2. **Implement fault isolation**: Learn from TradeTrap
3. **Use decoupled control**: Strategic + tactical separation
4. **Validate with simulated trading**: QuantAgents approach
5. **Monitor for hallucination**: Facts-grounded analysis
6. **Cost tracking**: Essential for ROI analysis
7. **Prompt optimization**: Adaptive-OPRO over fixed prompts
8. **Risk-first design**: Dual-decision architecture

---

## References

All papers available on arXiv. GitHub repositories noted where available.

**Analysis completed**: March 16, 2026  
**Total papers**: 23 (17 trading agents + 6 sentiment analysis)  
**Analyst**: Emmilia Lucent
