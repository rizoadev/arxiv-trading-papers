# Deep Analysis: Risk Management & Market Microstructure Papers

**Analysis Date:** March 16, 2026  
**Total Papers Analyzed:** 29  
**Categories:** Risk Management (10), Market Microstructure (10), Execution Algorithms (9)

---

## Executive Summary

This comprehensive analysis covers 29 recent arXiv papers spanning risk management, market microstructure, and execution algorithms. Key findings reveal a strong trend toward **RL-based approaches** for hedging and execution, **LLM integration** for risk-sensitive trading, and **generative models** for order book simulation. Production-ready implementations increasingly emphasize **compliance**, **auditability**, and **realistic market simulation**.

### Key Themes Across Categories:
1. **Reinforcement Learning Dominance**: 15+ papers use RL (PPO, DQN, actor-critic) for decision-making
2. **LLM Integration**: 4 papers incorporate LLMs for signal generation and risk assessment
3. **Generative Models**: Diffusion models and Transformers for LOB simulation and synthetic data
4. **Production Focus**: Emphasis on compliance, zero-knowledge audits, and realistic backtesting
5. **Direct Optimization**: Shift from intermediate predictions to direct objective optimization

---

# Part I: Risk Management Papers (10)

---

## 1. Deep Hedging with Reinforcement Learning: A Practical Framework for Option Risk Management

**arXiv:** 2512.12420 | **Date:** Dec 13, 2025 | **Authors:** Travon Lucius

### Core Information
- **Problem:** Dynamic hedging of equity index option exposures under realistic transaction costs and position limits
- **Key Contributions:** 
  - Leak-free RL environment for option hedging
  - Cost-aware reward function with stochastic actor-critic agent
  - Modular codebase extensible to multi-asset overlays

### Risk Management Details
- **Risk Measure:** Sharpe ratio optimization (mean-variance trade-off)
- **Risk Model:** Option surface and macro variables as state (volatility term structure, skew, realized vol, rates)
- **Hedging Strategy:** Dynamic delta hedging via SPY ETF trades
- **Optimization:** Actor-critic RL with generalized advantage estimation (GAE)
- **Constraints:** Position limits, transaction costs
- **Results:** Improved Sharpe vs no-hedge, momentum, vol-targeting baselines; robust to doubled transaction costs

### Code & Implementation
- **GitHub:** https://github.com/tlucius16/deep-hedging-rl
- **Framework:** Custom PyTorch (implied)
- **Data:** SPX/SPY daily end-of-day panel data

### Key Insights
- **Practical Applicability:** High - designed as portfolio overlay on existing SPX/SPY allocation
- **Production Considerations:** Controlled turnover, extensible to intraday data
- **Limitations:** Test-sample Sharpe only statistically distinguishable from zero for GAE policy
- **Integration:** Can sit on top of existing equity allocation as risk overlay

---

## 2. FinRL-DeepSeek: LLM-Infused Risk-Sensitive Reinforcement Learning for Trading Agents

**arXiv:** 2502.07393 | **Date:** Feb 11, 2025 | **Authors:** Mostapha Benhenda

### Core Information
- **Problem:** Integrating LLM-generated signals into risk-sensitive RL trading
- **Key Contributions:** 
  - Extension of CPPO (CVaR Proximal Policy Optimization) with LLM signals
  - Risk assessment from financial news using DeepSeek V3, Qwen 2.5, Llama 3.3

### Risk Management Details
- **Risk Measure:** Conditional Value-at-Risk (CVaR)
- **Risk Model:** LLM-generated risk assessment from financial news
- **Hedging Strategy:** Risk-sensitive position sizing via CPPO
- **Optimization:** CVaR-constrained PPO with LLM signal integration
- **Constraints:** Risk-sensitive action selection
- **Results:** Backtested on Nasdaq-100 with FNSPID news dataset

### Code & Implementation
- **GitHub:** https://github.com/benstaf/FinRL_DeepSeek
- **Framework:** FinRL + LLM APIs (DeepSeek, Qwen, Llama)
- **Data:** Nasdaq-100, FNSPID financial news dataset

### Key Insights
- **Practical Applicability:** Medium - requires LLM API integration and news data pipeline
- **Production Considerations:** LLM latency, news data freshness, model versioning
- **Limitations:** Backtested only on Nasdaq-100; LLM signal quality varies
- **Integration:** Can enhance existing RL trading systems with LLM risk signals

---

## 3. Autonomous AI Agents for Option Hedging: Enhancing Financial Stability through Shortfall Aware Reinforcement Learning

**arXiv:** 2603.06587 | **Date:** Feb 1, 2026 | **Authors:** Minxuan Hu

### Core Information
- **Problem:** Gap between static model calibration and realized hedging outcomes in derivatives markets
- **Key Contributions:**
  - Replication Learning of Option Pricing (RLOP) framework
  - Adaptive QLBS (Q-learner in Black-Scholes) extension
  - Shortfall probability and tail risk focus

### Risk Management Details
- **Risk Measure:** Shortfall probability, Expected Shortfall (ES)
- **Risk Model:** Realized path delta hedging outcome distributions
- **Hedging Strategy:** RL-based dynamic hedging with shortfall awareness
- **Optimization:** Q-learning with tail-risk sensitive rewards
- **Constraints:** Transaction costs, hedging frequency
- **Results:** RLOP reduces shortfall frequency; clearest tail-risk improvements in stress scenarios

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Custom RL (Q-learning based)
- **Data:** SPY and XOP listed options

### Key Insights
- **Practical Applicability:** High - addresses real derivatives hedging challenges
- **Production Considerations:** Implied vol fit may favor parametric models but poor after-cost performance
- **Limitations:** Friction-aware framework requires careful cost modeling
- **Integration:** Supports AI-augmented derivatives risk management at scale

---

## 4. FINRS: A Risk-Sensitive Trading Framework for Real Financial Markets

**arXiv:** 2511.12599 | **Date:** Nov 16, 2025 | **Authors:** Bijia Liu

### Core Information
- **Problem:** LLM-based trading agents lack integrated risk management mechanisms
- **Key Contributions:**
  - Hierarchical market analysis
  - Dual-decision agents (directional + risk-aware)
  - Multi-timescale reward reflection

### Risk Management Details
- **Risk Measure:** Downside risk constraints (specific measure not detailed in abstract)
- **Risk Model:** Hierarchical market analysis with risk integration
- **Hedging Strategy:** Dual-agent structure separates directional reasoning from position adjustment
- **Optimization:** Multi-timescale reward signals
- **Constraints:** Downside risk limits
- **Results:** Superior profitability and stability vs SOTA on multiple stocks

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** LLM-based agent system
- **Data:** Multiple stocks, various market conditions

### Key Insights
- **Practical Applicability:** High - addresses volatility market effectiveness
- **Production Considerations:** LLM inference latency, hierarchical analysis complexity
- **Limitations:** Single-step prediction focus in existing LLM agents addressed
- **Integration:** Framework for LLM-centered trading with built-in risk management

---

## 5. FinPos: A Position-Aware Trading Agent System for Real Financial Markets

**arXiv:** 2510.27251 | **Date:** Oct 31, 2025 (v2 Jan 7, 2026) | **Authors:** Bijia Liu

### Core Information
- **Problem:** Existing trading agents lack continuous position management awareness
- **Key Contributions:**
  - Position-aware trading task design
  - Three mechanisms: professional info interpretation, dual-agent decision, multi-timescale rewards
  - Long-term market decision-making focus

### Risk Management Details
- **Risk Measure:** Position risk (implicit in position adjustment mechanism)
- **Risk Model:** Continuous position state tracking
- **Hedging Strategy:** Dual-agent: directional reasoning + risk-aware position adjustment
- **Optimization:** Experiential feedback via multi-timescale rewards
- **Constraints:** Position limits (implicit)
- **Results:** Surpasses SOTA in position-aware trading task

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** LLM-centered agent system
- **Data:** Realistic market simulation

### Key Insights
- **Practical Applicability:** High - mirrors real market position management
- **Production Considerations:** Requires continuous position tracking infrastructure
- **Limitations:** Intraday independent unit-based trading is unrealistic
- **Integration:** LLM agents can handle long-term market decision-making

---

## 6. Taming Tail Risk in Financial Markets: Conformal Risk Control for Nonstationary Portfolio VaR

**arXiv:** 2602.03903 | **Date:** Feb 3, 2026 | **Authors:** Marc Schmitt

### Core Information
- **Problem:** Risk forecasts drive trading constraints but losses are nonstationary and regime-dependent
- **Key Contributions:**
  - Regime-weighted conformal risk control (RWC)
  - Model-agnostic wrapper for conditional quantile forecasters
  - Finite-sample coverage under weighted exchangeability

### Risk Management Details
- **Risk Measure:** Value-at-Risk (VaR)
- **Risk Model:** Regime-weighted conformal calibration with exponential time decay
- **Hedging Strategy:** Sequential one-sided VaR control via safety buffer calibration
- **Optimization:** Conformal prediction with regime-similarity weights
- **Constraints:** Target exceedance rate
- **Results:** Time-weighted conformal calibration strong default under drift; regime weighting improves conditional stability

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Conformal prediction (model-agnostic)
- **Data:** CRSP U.S. equity portfolio

### Key Insights
- **Practical Applicability:** High - wraps any quantile forecaster
- **Production Considerations:** Regime feature selection, calibration frequency
- **Limitations:** Approximation bounds under smoothly drifting regimes
- **Integration:** Can enhance existing VaR models with conformal guarantees

---

## 7. Coherent Estimation of Risk Measures

**arXiv:** 2510.05809 | **Date:** Oct 7, 2025 (v3 Dec 3, 2025) | **Authors:** Damian Jelito

### Core Information
- **Problem:** Statistical framework for risk estimation aligned with axiomatic risk measure theory
- **Key Contributions:**
  - Coherent risk estimators via robust representations
  - Link to L-estimators
  - Canonical methodology for capital adequacy

### Risk Management Details
- **Risk Measure:** Expected Shortfall (ES) - focus on FRTB regulatory applications
- **Risk Model:** Statistical framework inheriting economic properties of risk measures
- **Hedging Strategy:** Not applicable (estimation framework)
- **Optimization:** Robust estimation under i.i.d. and overlapping samples
- **Constraints:** Regulatory FRTB model requirements
- **Results:** Numerical study illustrates ES estimation approach

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Statistical estimation (R/Python implied)
- **Data:** P&L samples (regulatory context)

### Key Insights
- **Practical Applicability:** High for regulatory risk management
- **Production Considerations:** FRTB compliance, overlapping sample handling
- **Limitations:** Theoretical framework requires implementation
- **Integration:** Unifies risk measure theory with practical statistical challenges

---

## 8. Static Marginal Expected Shortfall: Systemic Risk Measurement under Dependence Uncertainty

**arXiv:** 2504.19953 | **Date:** Apr 28, 2025 (v2 Nov 14, 2025) | **Authors:** Jinghui Chen

### Core Information
- **Problem:** Measuring bank/insurance contribution to systemic risk under dependence uncertainty
- **Key Contributions:**
  - Worst-case/best-case bounds for Marginal Expected Shortfall (MES)
  - Tighter bounds with partial dependence information
  - Three factor models: additive, minimum-based, multiplicative

### Risk Management Details
- **Risk Measure:** Marginal Expected Shortfall (MES)
- **Risk Model:** Factor models (additive, min-based, multiplicative background risk)
- **Hedging Strategy:** Not applicable (systemic risk measurement)
- **Optimization:** Bound derivation under dependence uncertainty
- **Constraints:** Known individual risk distributions, unknown dependence
- **Results:** Empirical analysis demonstrates practical relevance for practitioners/policymakers

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Statistical/actuarial modeling
- **Data:** Bank/insurance industry data (empirical analysis)

### Key Insights
- **Practical Applicability:** High for systemic risk monitoring
- **Production Considerations:** Factor model selection, partial information incorporation
- **Limitations:** Bounds may be wide without dependence information
- **Integration:** CAPM/Weighted Insurance Pricing Model consistent linear relationship

---

## 9. EVT-Based Rate-Preserving Distributional Robustness for Tail Risk Functionals

**arXiv:** 2506.16230 | **Date:** Jun 19, 2025 (v2 Jan 21, 2026) | **Authors:** Anand Deo

### Core Information
- **Problem:** Tail risk measures focus on extreme losses where scarce data makes model error unavoidable
- **Key Contributions:**
  - First-order asymptotics for worst-case tail risk under ambiguity sets
  - Tail-calibrated ambiguity design preserving nominal tail scaling
  - Avoids severe risk inflation from standard ambiguity sets

### Risk Management Details
- **Risk Measure:** Conditional Value-at-Risk (CVaR) and broader tail-risk measures
- **Risk Model:** Extreme Value Theory (EVT) with distributional robustness
- **Hedging Strategy:** Not applicable (risk estimation)
- **Optimization:** Worst-case risk over Wasserstein balls and φ-divergence neighborhoods
- **Constraints:** Tail level β→0 asymptotics
- **Results:** Plug-in implementation preserves baseline first-order scaling; avoids severe inflation

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** EVT + distributionally robust optimization
- **Data:** Synthetic and real-data experiments

### Key Insights
- **Practical Applicability:** High for tail risk estimation
- **Production Considerations:** Tail-index estimation consistency, tuning parameter selection
- **Limitations:** Standard ambiguity sets can cause excess risk inflation
- **Integration:** Tail-calibrated design guards against misspecification

---

## 10. Deep Declarative Risk Budgeting Portfolios

**arXiv:** 2504.19980 | **Date:** Apr 28, 2025 | **Authors:** Manuel Parra-Díaz

### Core Information
- **Problem:** End-to-end deep learning frameworks for portfolio optimization sensitive to initialization
- **Key Contributions:**
  - Robust end-to-end framework for risk budgeting portfolios
  - Reduced sensitivity to neural network initialization
  - Consistent outperformance vs risk parity benchmark

### Risk Management Details
- **Risk Measure:** Risk budgeting (implicit - portfolio risk allocation)
- **Risk Model:** Deep declarative networks with implicit layers
- **Hedging Strategy:** Portfolio allocation via risk budgeting
- **Optimization:** End-to-end learning with implicit layers
- **Constraints:** Risk budget constraints
- **Results:** Enhanced stability without compromising performance; beats risk parity

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Deep learning (PyTorch/TensorFlow implied)
- **Data:** Portfolio returns (not specified)

### Key Insights
- **Practical Applicability:** Medium-High for portfolio construction
- **Production Considerations:** Initialization sensitivity addressed, but framework complexity
- **Limitations:** Performance consistency was primary issue addressed
- **Integration:** End-to-end framework for risk budgeting portfolio optimization

---

# Part II: Market Microstructure Papers (10)

---

## 11. Limit Order Book Dynamics in Matching Markets: Microstructure, Spread, and Execution Slippage

**arXiv:** 2511.20606 | **Date:** Nov 25, 2025 (v2 Nov 26, 2025) | **Authors:** Yao Wu

### Core Information
- **Problem:** Monetary transfers often fail to close structural preference gaps in matching markets
- **Key Contributions:**
  - LOB framework for matching decisions with rigid bid-ask spreads
  - Threshold Impossibility Theorem
  - Dynamic discrete choice execution model

### Market Microstructure Details
- **Order Book Model:** Latent preference state matrix with bid-ask spread representation
- **Market Dynamics:** Threshold-based execution under inventory pressure
- **Features:** Market-to-book ratio, time-decaying liquidity threshold
- **Prediction Target:** Match execution probability
- **Model Architecture:** Dynamic discrete choice model
- **Simulation Fidelity:** Numerical experiments validate persistent slippage, regional invariance

### Code & Implementation
- **GitHub:** https://github.com/Republic1024/Limit-Order-Matching-Microstructure
- **Framework:** Not specified
- **Data:** Not specified (numerical experiments)

### Key Insights
- **Practical Applicability:** Medium - theoretical framework for matching markets
- **Production Considerations:** Threshold calibration, spread modeling
- **Limitations:** Linear compensation cannot close spreads without identity shift
- **Integration:** Microstructure explanation for matching failures and post-match regret

---

## 12. Deep Learning Meets Queue-Reactive: A Framework for Realistic Limit Order Book Simulation

**arXiv:** 2501.08822 | **Date:** Jan 15, 2025 | **Authors:** Hamza Bodor

### Core Information
- **Problem:** Queue-Reactive model assumptions limit realism (queue independence, limited state space)
- **Key Contributions:**
  - Multidimensional Deep Queue-Reactive (MDQR) model
  - Relaxes queue independence, enriches state space, models order size distribution
  - Neural network learns cross-queue dependencies

### Market Microstructure Details
- **Order Book Model:** MDQR - extends Queue-Reactive (Huang et al. 2015)
- **Market Dynamics:** Cross-queue correlations, varying market conditions
- **Features:** Market features, order size distributions, multi-level dependencies
- **Prediction Target:** Order arrival/cancellation at multiple price levels
- **Model Architecture:** Neural network with point-process foundation
- **Simulation Fidelity:** Captures square-root law, cross-queue correlations, realistic order sizes

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Deep learning + point processes
- **Data:** Bund futures market

### Key Insights
- **Practical Applicability:** High - maintains computational efficiency for RL/backtesting
- **Production Considerations:** Neural network training, state space design
- **Limitations:** Bund futures only (may not generalize)
- **Integration:** Suitable for strategy development via RL or realistic backtesting

---

## 13. ClusterLOB: Enhancing Trading Strategies by Clustering Orders in Limit Order Books

**arXiv:** 2504.20349 | **Date:** Apr 29, 2025 (v3 May 10, 2025) | **Authors:** Yichi Zhang

### Core Information
- **Problem:** Understanding LOB dynamics and participant behavior for trading strategy enhancement
- **Key Contributions:**
  - K-means++ clustering of market-by-order (MBO) events
  - Three clusters: directional, opportunistic, market-making
  - Order flow imbalance signals per cluster

### Market Microstructure Details
- **Order Book Model:** Market-by-order (MBO) data with 6 time-dependent features
- **Market Dynamics:** Participant behavior clustering
- **Features:** Order flow imbalance per cluster (30-minute buckets)
- **Prediction Target:** Trading signal generation
- **Model Architecture:** K-means++ clustering
- **Simulation Fidelity:** One year NASDAQ data (small/medium/large-tick stocks)

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** K-means++ clustering
- **Data:** NASDAQ MBO data (1 year)

### Key Insights
- **Practical Applicability:** High - validated with superior test performance
- **Production Considerations:** Cluster identification, feature engineering
- **Limitations:** Requires MBO data (more granular than MBP)
- **Integration:** Robust framework for predictive trading signals in algo trading

---

## 14. DiffVolume: Diffusion Models for Volume Generation in Limit Order Books

**arXiv:** 2508.08698 | **Date:** Aug 12, 2025 | **Authors:** Zhuohan Wang

### Core Information
- **Problem:** Generating high-dimensional LOB volume snapshots with temporal/liquidity patterns
- **Key Contributions:**
  - Conditional diffusion model for LOB volume generation
  - Realism, counterfactual generation, downstream prediction evaluation
  - Controllable generation under hypothetical liquidity scenarios

### Market Microstructure Details
- **Order Book Model:** Conditional diffusion model for volume snapshots
- **Market Dynamics:** Temporal and liquidity-dependent patterns
- **Features:** Past volume history, time of day, target liquidity profile
- **Prediction Target:** Future LOB volume snapshots
- **Model Architecture:** Diffusion model (conditional)
- **Simulation Fidelity:** Better reproduces marginal distribution, spatial correlation, autocorrelation decay

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Diffusion models (PyTorch implied)
- **Data:** LOB data (not specified)

### Key Insights
- **Practical Applicability:** High for synthetic data generation
- **Production Considerations:** Diffusion model training time, conditioning design
- **Limitations:** High-dimensional generation complexity
- **Integration:** Synthetic counterfactual data improves liquidity forecasting models

---

## 15. Event-Based Limit Order Book Simulation under a Neural Hawkes Process: Application in Market-Making

**arXiv:** 2502.17417 | **Date:** Feb 24, 2025 | **Authors:** Luca Lalor

### Core Information
- **Problem:** Capturing high-frequency LOB dynamics for realistic market-making simulation
- **Key Contributions:**
  - Event-driven LOB model with 12 LOB events
  - Neural Hawkes process with LSTM for event relationships
  - Application to DRL market-making framework

### Market Microstructure Details
- **Order Book Model:** Neural Hawkes process for 12 event types
- **Market Dynamics:** Long- and short-term event interactions
- **Features:** Event types, timing, midprice process
- **Prediction Target:** Event arrival and midprice evolution
- **Model Architecture:** Neural Hawkes with LSTM
- **Simulation Fidelity:** Captures price fluctuation volatility; aligns with real data on order fills

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Neural Hawkes + DRL
- **Data:** Exchange-based financial markets

### Key Insights
- **Practical Applicability:** High for market-making strategy development
- **Production Considerations:** Event type definition, LSTM training
- **Limitations:** 12 events may not capture all market behaviors
- **Integration:** DRL market-making agent can train on simulated data

---

## 16. TradeFM: A Generative Foundation Model for Trade-flow and Market Microstructure

**arXiv:** 2602.23784 | **Date:** Feb 27, 2026 | **Authors:** Srijan Sood

### Core Information
- **Problem:** Lack of general-purpose representations for market microstructure across assets
- **Key Contributions:**
  - 524M-parameter generative Transformer for market microstructure
  - Scale-invariant features and universal tokenization
  - Cross-asset generalization to >9K equities

### Market Microstructure Details
- **Order Book Model:** Generative Transformer for trade-flow
- **Market Dynamics:** Billions of trade events across heterogeneous markets
- **Features:** Scale-invariant trade representations
- **Prediction Target:** Trade event sequences
- **Model Architecture:** 524M-parameter Transformer
- **Simulation Fidelity:** Reproduces heavy tails, volatility clustering, no return autocorrelation; 2-3x lower error than Compound Hawkes

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Transformer (PyTorch/JAX implied)
- **Data:** >9K equities, billions of trade events

### Key Insights
- **Practical Applicability:** High for synthetic data and stress testing
- **Production Considerations:** Model size (524M params), cross-asset calibration
- **Limitations:** Zero-shot APAC generalization shows moderate perplexity degradation
- **Integration:** Foundation model for learning-based trading agents

---

## 17. A Unified Theory of Order Flow, Market Impact, and Volatility

**arXiv:** 2601.23172 | **Date:** Jan 30, 2026 (v2 Feb 2, 2026) | **Authors:** Grégoire Szymanski

### Core Information
- **Problem:** Reconciling persistent order flow, rough volatility, and power-law market impact
- **Key Contributions:**
  - Microstructural model distinguishing core orders and reaction flow
  - Single statistic H₀ pins down all quantities
  - Scaling limit reconciles empirical properties

### Market Microstructure Details
- **Order Book Model:** Hawkes processes for core orders and reaction flow
- **Market Dynamics:** Persistent signed order flow, rough volume/volatility
- **Features:** H₀ (persistence of core flow) ≈ 3/4
- **Prediction Target:** Market impact exponent, volatility roughness
- **Model Architecture:** Hawkes process with scaling limit
- **Simulation Fidelity:** Matches square-root law, rough volatility estimates

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Hawkes processes, fractional processes
- **Data:** Signed order flow data

### Key Insights
- **Practical Applicability:** High - unified theoretical framework
- **Production Considerations:** H₀ estimation, no-arbitrage constraints
- **Limitations:** Theoretical model requires empirical calibration
- **Integration:** Explains universality of square-root impact law

---

## 18. Generating Realistic Metaorders from Public Data

**arXiv:** 2503.18199 | **Date:** Mar 23, 2025 (v2 Apr 5, 2025) | **Authors:** Guillaume Maitrier

### Core Information
- **Problem:** Price impact research relies on proprietary datasets, limiting reproducibility
- **Key Contributions:**
  - Novel algorithm for metaorder generation from public trade data
  - Recovers all established stylized facts (Square Root Law, concave profile, decay)
  - Enables larger, more robust datasets

### Market Microstructure Details
- **Order Book Model:** Metaorder reconstruction algorithm
- **Market Dynamics:** Price impact from metaorder execution
- **Features:** Public trade data, metaorder signatures
- **Prediction Target:** Metaorder identification and impact
- **Model Architecture:** Algorithmic reconstruction
- **Simulation Fidelity:** Recovers Square Root Law, concave profile, post-execution decay

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Algorithmic (not ML)
- **Data:** Public trade data

### Key Insights
- **Practical Applicability:** High - overcomes proprietary data barrier
- **Production Considerations:** Algorithm parameters, data quality
- **Limitations:** Public data may miss some metaorder signals
- **Integration:** Suggests mechanical (not informational) origin of Square Root Law

---

## 19. Metaorder Modelling and Identification from Public Data

**arXiv:** 2602.19590 | **Date:** Feb 23, 2026 | **Authors:** Ezra Goliath

### Core Information
- **Problem:** LMF order-splitting theory tests rely on proprietary trader identifiers
- **Key Contributions:**
  - Validation of LMF theory using public JSE data
  - Synthetic metaorder reconstruction methods
  - Cross-market validation capability

### Market Microstructure Details
- **Order Book Model:** LMF order-splitting theory validation
- **Market Dynamics:** Long-range correlations in market-order flow
- **Features:** N=50 or N=150 effective traders assumption
- **Prediction Target:** Metaorder identification
- **Model Architecture:** Synthetic metaorder reconstruction
- **Simulation Fidelity:** 3 years TAQ data for JSE top 100 stocks

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Metaorder reconstruction
- **Data:** JSE TAQ data (3 years, top 100 stocks)

### Key Insights
- **Practical Applicability:** High for cross-market metaorder analysis
- **Production Considerations:** N (effective traders) selection, data granularity
- **Limitations:** JSE-specific validation (may not generalize)
- **Integration:** Enables LMF theory testing without proprietary data

---

## 20. Temporal Kolmogorov-Arnold Networks (T-KAN) for High-Frequency Limit Order Book Forecasting

**arXiv:** 2601.02310 | **Date:** Jan 5, 2026 | **Authors:** Ahmad Makinde

### Core Information
- **Problem:** Alpha decay in HFT - traditional models (DeepLOB) lose predictive power as horizon increases
- **Key Contributions:**
  - T-KAN architecture replacing LSTM fixed weights with learnable B-spline activations
  - 19.1% F1-score improvement at k=100 horizon
  - FPGA-optimized for low-latency

### Market Microstructure Details
- **Order Book Model:** T-KAN for LOB forecasting
- **Market Dynamics:** High-frequency noisy non-linear patterns
- **Features:** LOB data from FI-2010 dataset
- **Prediction Target:** Mid-price direction (k=100 horizon)
- **Model Architecture:** Temporal KAN with B-spline activations
- **Simulation Fidelity:** FI-2010 dataset; 132.48% return vs -82.76% DeepLOB drawdown

### Code & Implementation
- **GitHub:** https://github.com/AhmadMak/Temporal-Kolmogorov-Arnold-Networks-T-KAN-for-High-Frequency-Limit-Order-Book-Forecasting
- **Framework:** T-KAN (custom)
- **Data:** FI-2010 dataset

### Key Insights
- **Practical Applicability:** High for HFT - FPGA optimized via HLS
- **Production Considerations:** B-spline training, interpretability (dead-zones visible)
- **Limitations:** FI-2010 dataset may not represent all markets
- **Integration:** Low-latency FPGA implementation ready

---

# Part III: Execution Algorithms (9)

---

## 21. Deep Learning for VWAP Execution in Crypto Markets: Beyond the Volume Curve

**arXiv:** 2502.13722 | **Date:** Feb 19, 2025 (v2 Apr 2, 2025) | **Authors:** Remi Genet

### Core Information
- **Problem:** Traditional VWAP approaches focus on volume curve prediction, suboptimal in volatile markets
- **Key Contributions:**
  - Direct VWAP slippage optimization via deep learning
  - Bypasses intermediate volume curve prediction
  - Custom loss functions with automatic differentiation

### Execution Details
- **Execution Objective:** VWAP slippage minimization
- **Order Types:** Not specified (implied market orders)
- **Execution Algorithm:** Deep learning with direct objective optimization
- **Market Impact Model:** Implicit in slippage minimization
- **Latency Considerations:** Not specified
- **Performance:** Lower VWAP slippage vs conventional methods; strategies diverge from volume curve predictions

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Deep learning (PyTorch/TensorFlow implied)
- **Data:** Cryptocurrency markets (hourly)

### Key Insights
- **Practical Applicability:** High for crypto, applicable to equities
- **Production Considerations:** Direct optimization avoids volume prediction errors
- **Limitations:** Crypto-focused (high volatility)
- **Integration:** Framework readily applicable to other asset classes

---

## 22. RL-Exec: Impact-Aware Reinforcement Learning for Opportunistic Optimal Liquidation

**arXiv:** 2511.07434 | **Date:** Oct 30, 2025 | **Authors:** Enzo Duflot

### Core Information
- **Problem:** Opportunistic optimal liquidation with realistic market frictions
- **Key Contributions:**
  - PPO agent trained on historical replays with endogenous impact
  - Strict time-split evaluation with per-day protocol
  - Statistical significance testing

### Execution Details
- **Execution Objective:** Optimal liquidation over fixed deadlines
- **Order Types:** Limit orders (partial fills modeled)
- **Execution Algorithm:** PPO with depth-20 LOB features
- **Market Impact Model:** Transient impact with resilience
- **Latency Considerations:** Latency modeled in simulation
- **Performance:** +2-3 bps at 30min, +7-8 bps at 60min, +23 bps at 120min vs TWAP/VWAP (statistically significant)

### Code & Implementation
- **GitHub:** http://github.com/Giafferri/RL-Exec
- **Framework:** PPO (Stable Baselines implied)
- **Data:** BTC-USD LOB (Jan-Feb 2020)

### Key Insights
- **Practical Applicability:** High - realistic BTC-USD replay evaluation
- **Production Considerations:** Maker/taker fees, partial fills, latency
- **Limitations:** BTC-USD only (crypto-specific)
- **Integration:** Outperforms TWAP and book-liquidity VWAP significantly

---

## 23. Reinforcement Learning for Optimal Execution when Liquidity is Time-Varying

**arXiv:** 2402.12049 | **Date:** Feb 19, 2024 (v2 Feb 20, 2024) | **Authors:** Andrea Macrì

### Core Information
- **Problem:** Optimal execution models assume constant market impact; liquidity is dynamic and latent
- **Key Contributions:**
  - Double Deep Q-learning for time-varying liquidity
  - Almgren-Chriss framework with stochastic impact parameters
  - Learns optimal policy when analytical solution unavailable

### Execution Details
- **Execution Objective:** Optimal execution with time-varying liquidity
- **Order Types:** Not specified (implied market orders)
- **Execution Algorithm:** Double Deep Q-learning
- **Market Impact Model:** Almgren-Chriss with temporary/permanent impact (stochastic)
- **Latency Considerations:** Not specified
- **Performance:** Learns optimal policy; overcomes benchmarks when solution unavailable

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Double DQN
- **Data:** Numerical experiments (simulated)

### Key Insights
- **Practical Applicability:** High - addresses latent liquidity challenge
- **Production Considerations:** Liquidity inference, impact parameter estimation
- **Limitations:** Simulated data (needs real-market validation)
- **Integration:** Extends Almgren-Chriss to realistic time-varying setting

---

## 24. Imitate then Transcend: Multi-Agent Optimal Execution with Dual-Window Denoise PPO

**arXiv:** 2206.10736 | **Date:** Jun 21, 2022 | **Authors:** Jin Fang

### Core Information
- **Problem:** Optimal execution and placement with noisy market data
- **Key Contributions:**
  - Dual-window Denoise PPO architecture
  - Imitation learning reward scheme
  - Multi-agent realistic historical LOB simulator

### Execution Details
- **Execution Objective:** Optimal execution and placement (combined)
- **Order Types:** Market and limit orders (flexible action formulation)
- **Execution Algorithm:** Dual-Window Denoise PPO with imitation learning
- **Market Impact Model:** Accurate assessment in multi-agent simulator
- **Latency Considerations:** Not specified
- **Performance:** Consistently outperforms TWAP; generalizes across dates/tickers

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** PPO with imitation learning
- **Data:** Historical LOB (multi-agent simulator)

### Key Insights
- **Practical Applicability:** High - industry benchmark outperformance
- **Production Considerations:** Imitation learning requires expert demonstrations
- **Limitations:** 2022 paper (older but foundational)
- **Integration:** Flexible action formulation handles execution + placement jointly

---

## 25. Reinforcement Learning for Trade Execution with Market and Limit Orders

**arXiv:** 2507.06345 | **Date:** Jul 8, 2025 (v2 Jan 26, 2026) | **Authors:** Moritz Weiss

### Core Information
- **Problem:** Optimal trade execution in LOB with market and limit order allocation
- **Key Contributions:**
  - Dynamic allocation framework for market/limit orders
  - Multivariate logistic-normal distribution modeling
  - Efficient RL training

### Execution Details
- **Execution Objective:** Maximize expected revenue
- **Order Types:** Market and limit orders
- **Execution Algorithm:** RL with multivariate logistic-normal action distribution
- **Market Impact Model:** Simulated LOB with noise/tactical/strategic traders
- **Latency Considerations:** Not specified
- **Performance:** Outperforms traditional benchmarks in simulation

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Custom RL
- **Data:** Simulated LOB environment

### Key Insights
- **Practical Applicability:** Medium-High - simulation validated
- **Production Considerations:** Logistic-normal parameterization enables efficient training
- **Limitations:** Simulated environment (needs real-market testing)
- **Integration:** Unified framework for market/limit order allocation

---

## 26. Right Place, Right Time: Market Simulation-based RL for Execution Optimisation

**arXiv:** 2510.22206 | **Date:** Oct 25, 2025 | **Authors:** Namid Stillman

### Core Information
- **Problem:** Optimizing sophisticated execution algorithms is increasingly challenging
- **Key Contributions:**
  - RL framework evaluated in reactive agent-based market simulator
  - Slippage decomposition: market impact + execution risk
  - Efficient frontier assessment (Almgren-Chriss)

### Execution Details
- **Execution Objective:** Balance risk and cost (efficient frontier)
- **Order Types:** Not specified
- **Execution Algorithm:** RL in reactive agent-based simulator
- **Market Impact Model:** Reactive order flow with slippage decomposition
- **Latency Considerations:** Not specified
- **Performance:** Consistently outperforms baselines; operates near efficient frontier

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** RL + agent-based simulation
- **Data:** Simulated (reactive agent-based)

### Key Insights
- **Practical Applicability:** High - ICAIF 2025 accepted
- **Production Considerations:** Simulator fidelity critical
- **Limitations:** Simulation-based (needs real-market validation)
- **Integration:** RL strategies near Almgren-Chriss efficient frontier

---

## 27. VWAP Execution with Signature-Enhanced Transformers: A Multi-Asset Learning Approach

**arXiv:** 2503.02680 | **Date:** Mar 4, 2025 | **Authors:** Remi Genet

### Core Information
- **Problem:** Asset-specific model training and complex temporal dependencies in VWAP execution
- **Key Contributions:**
  - Single neural network trained across multiple assets
  - Transformer + path signatures for geometric features
  - Global fitting with signature features (GFT-Sig)

### Execution Details
- **Execution Objective:** VWAP loss minimization (absolute and quadratic)
- **Order Types:** Not specified
- **Execution Algorithm:** Signature-enhanced Transformer
- **Market Impact Model:** Implicit in VWAP loss
- **Latency Considerations:** Not specified
- **Performance:** Superior vs asset-specific models; generalizes to out-of-sample assets

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Transformer + path signatures
- **Data:** 80 cryptocurrency trading pairs (hourly)

### Key Insights
- **Practical Applicability:** High - scalable multi-asset approach
- **Production Considerations:** Signature feature computation, global training
- **Limitations:** Crypto-focused
- **Integration:** Global parameter sharing + signatures = robust VWAP execution

---

## 28. Hybrid Genetic Algorithm for Optimal User Order Routing

**arXiv:** 2510.21647 | **Date:** Oct 24, 2025 | **Authors:** Mitchell Marfinetz

### Core Information
- **Problem:** DEX routing in CoW Protocol batch auctions - deterministic heuristics fail on split-flow opportunities
- **Key Contributions:**
  - Hybrid GA with NSGA-II multi-objective optimization
  - Pareto objective: (user surplus, -gas, -slippage, -risk)
  - 8/8 safety tests passed

### Execution Details
- **Execution Objective:** Maximize user surplus across AMMs
- **Order Types:** DEX swaps (AMM routing)
- **Execution Algorithm:** Hybrid GA + deterministic dual-decomposition
- **Market Impact Model:** Slippage, gas, execution risk
- **Latency Considerations:** 0.5s median convergence (2s limit)
- **Performance:** +0.40-9.82 ETH on small-medium orders; large orders unprofitable

### Code & Implementation
- **GitHub:** Open-source (not specified)
- **Framework:** NSGA-II + Bellman-Ford
- **Data:** CoW Protocol batch auctions

### Key Insights
- **Practical Applicability:** High for DEX routing
- **Production Considerations:** 6 protection layers, anytime operation
- **Limitations:** Large high-fragmentation orders unprofitable
- **Integration:** First openly documented multi-objective GA for real-time DEX routing

---

## 29. Safe and Compliant Cross-Market Trade Execution via Constrained RL and Zero-Knowledge Audits

**arXiv:** 2510.04952 | **Date:** Oct 6, 2025 (v2 Oct 7, 2025) | **Authors:** Ailiya Borjigin

### Core Information
- **Problem:** Balancing execution quality with rigorous compliance enforcement in cross-market trading
- **Key Contributions:**
  - Constrained MDP with hard constraints (participation limits, price bands, self-trading avoidance)
  - Runtime action-shield for unsafe action projection
  - Zero-knowledge compliance audit layer

### Execution Details
- **Execution Objective:** Minimize implementation shortfall and variance
- **Order Types:** Not specified (multi-venue)
- **Execution Algorithm:** PPO with constrained RL + action shield
- **Market Impact Model:** ABIDES-based multi-venue simulator
- **Latency Considerations:** Elevated latency stress tested
- **Performance:** Reduces shortfall/variance; zero constraint violations across stress scenarios

### Code & Implementation
- **GitHub:** Not specified
- **Framework:** Constrained PPO + zero-knowledge proofs
- **Data:** ABIDES multi-venue simulator

### Key Insights
- **Practical Applicability:** High for regulated environments
- **Production Considerations:** ZK audit overhead, constraint definition
- **Limitations:** Modeling assumptions, computational overhead
- **Integration:** Intersection of optimal execution, safe RL, RegTech, verifiable AI

---

# Synthesis: Production Implementation Guidelines

## Risk Management in Production

### Best Practices from Papers:
1. **Conformal Risk Control** (Paper 6): Wrap existing VaR models with conformal calibration for finite-sample guarantees
2. **LLM-Enhanced Risk Signals** (Papers 2, 4, 5): Integrate LLM-generated risk assessments but manage latency
3. **Tail-Risk Focus** (Papers 3, 8, 9): Use Expected Shortfall over VaR; apply EVT for tail calibration
4. **Position Awareness** (Paper 5): Track continuous positions, not just intraday signals
5. **Robust Estimation** (Papers 7, 9): Use coherent estimators; guard against distributional uncertainty

### Recommended Architecture:
```
Risk Management Stack:
├── VaR/ES Estimation (conformal-calibrated)
├── LLM Risk Signal Generator (async, cached)
├── Tail Risk Monitor (EVT-based)
├── Position Tracker (continuous state)
└── Action Shield (constraint enforcement)
```

---

## Order Book Modeling Best Practices

### Key Insights:
1. **Queue-Reactive Extensions** (Paper 12): MDQR captures cross-queue correlations; suitable for RL training
2. **Event-Based Simulation** (Paper 15): Neural Hawkes for 12+ event types; realistic market-making training
3. **Generative Models** (Papers 14, 16): Diffusion models for volume; Transformers for trade-flow (TradeFM)
4. **Participant Clustering** (Paper 13): K-means++ on MBO data reveals directional/opportunistic/MM behavior
5. **Metaorder Reconstruction** (Papers 18, 19): Generate realistic metaorders from public data

### Recommended Approach:
```
LOB Simulation Stack:
├── Base Model: MDQR or Neural Hawkes
├── Volume Generation: DiffVolume (diffusion)
├── Trade Flow: TradeFM (if scale justifies 524M params)
├── Metaorders: Public data reconstruction
└── Validation: Stylized facts (square-root law, volatility clustering)
```

---

## Execution Algorithm Design Patterns

### Patterns from Papers:
1. **Direct Objective Optimization** (Papers 21, 27): Optimize VWAP slippage directly, not via volume prediction
2. **Impact-Aware RL** (Papers 22, 23, 26): Model transient impact with resilience; use LOB features
3. **Multi-Asset Learning** (Paper 27): Global fitting with signature features generalizes across assets
4. **Constrained Execution** (Paper 29): Hard constraints with action shields + ZK audits for compliance
5. **Hybrid Approaches** (Paper 28): GA + deterministic fallback for DEX routing

### Recommended Architecture:
```
Execution Stack:
├── Planner: High-level execution schedule (TWAP/VWAP/IS)
├── RL Agent: PPO/DQN with LOB features + impact model
├── Action Shield: Constraint enforcement (price bands, participation limits)
├── Compliance: Zero-knowledge audit layer (if regulated)
└── Fallback: Deterministic algorithm (TWAP/VWAP)
```

---

## Market Impact Estimation

### Methods from Papers:
1. **Square-Root Law** (Papers 17, 18, 19): Universal impact law; H₀ ≈ 3/4 from order flow persistence
2. **Transient Impact** (Paper 22): Model resilience (decay) after trades
3. **Almgren-Chriss Extension** (Paper 23): Time-varying temporary/permanent impact
4. **Mechanical Origin** (Paper 18): Impact not from information but mechanical order flow

### Implementation:
```python
# Simplified impact model (from Paper 17, 22)
def market_impact(order_size, liquidity, H0=0.75):
    """Square-root impact with transient decay"""
    permanent = order_size ** (2 - 2*H0)  # ≈ sqrt(order_size)
    transient = order_size * np.exp(-decay_rate * time)
    return permanent + transient
```

---

## Integration with Trading Bot

### Recommended Integration Points:

1. **Pre-Trade Risk Check:**
   - VaR/ES limits (conformal-calibrated)
   - Position limits
   - Compliance constraints

2. **Execution Decision:**
   - RL agent with LOB features
   - Market/limit order allocation
   - Impact-aware scheduling

3. **Post-Trade Analysis:**
   - Slippage decomposition (impact vs timing)
   - Constraint violation audit (ZK proofs if needed)
   - Model retraining signals

4. **Continuous Monitoring:**
   - Tail risk alerts (EVT-based)
   - Regime detection (for conformal calibration)
   - LLM risk signal updates

---

## Code Repositories Summary

| Paper | GitHub | Framework |
|-------|--------|-----------|
| 1 (Deep Hedging) | github.com/tlucius16/deep-hedging-rl | PyTorch (implied) |
| 2 (FinRL-DeepSeek) | github.com/benstaf/FinRL_DeepSeek | FinRL + LLM APIs |
| 11 (LOB Matching) | github.com/Republic1024/Limit-Order-Matching-Microstructure | - |
| 20 (T-KAN) | github.com/AhmadMak/Temporal-KAN-LOB | Custom T-KAN |
| 22 (RL-Exec) | github.com/Giafferri/RL-Exec | PPO |

---

## Limitations & Future Work

### Common Limitations:
1. **Simulation vs Reality**: Many papers use simulated data; real-market validation needed
2. **Crypto Focus**: Several papers focus on crypto (high volatility, 24/7 markets)
3. **Latency**: HFT papers (T-KAN) address latency; most RL papers don't
4. **Transaction Costs**: Often simplified; real fees (maker/taker, gas) vary
5. **Regulatory**: Only Paper 29 addresses compliance rigorously

### Future Directions:
1. **Multi-Agent Systems**: Papers 24, 26 show promise for multi-agent execution
2. **Foundation Models**: TradeFM (Paper 16) suggests scaling benefits
3. **Verifiable AI**: ZK audits (Paper 29) for regulatory compliance
4. **Cross-Asset Generalization**: Signature-enhanced models (Paper 27)
5. **LLM + RL Integration**: Papers 2, 4, 5 show early LLM-RL fusion

---

**Analysis Complete.** 29 papers analyzed across Risk Management (10), Market Microstructure (10), and Execution Algorithms (9).

**Key Takeaway:** The field is moving toward **production-ready RL systems** with **compliance guarantees**, **realistic simulation**, and **direct objective optimization**. LLM integration is emerging but requires careful latency management. Generative models (diffusion, Transformers) enable synthetic data generation for training and stress testing.
