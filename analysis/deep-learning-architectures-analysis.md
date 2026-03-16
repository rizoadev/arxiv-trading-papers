# Deep Learning & Advanced Architectures Analysis for Trading

**Analysis Date**: March 16, 2026  
**Total Papers Analyzed**: 29  
**Categories**: Deep Learning for Markets (12), GNN & Advanced Architectures (8), Volatility & Statistical Arbitrage (9)

---

## Executive Summary

This comprehensive analysis covers 29 cutting-edge research papers on deep learning and advanced architectures for financial trading. Key findings include:

- **Transformer Dominance**: Transformers and hybrid Transformer architectures consistently outperform traditional models across most tasks
- **GNN Integration**: Graph Neural Networks show significant promise for capturing inter-asset relationships
- **Mamba Emergence**: New Mamba-based architectures offer near-linear complexity with competitive accuracy
- **Diffusion Models**: Emerging application of diffusion models for LOB volume generation shows strong results
- **Causal Methods**: Integration of causal inference with deep learning represents the next frontier
- **Production Readiness**: Most models remain research-grade; few provide production-ready code

---

# SECTION 1: Deep Learning for Markets (12 Papers)

---

## Paper 1: Minimal Batch Adaptive Learning Policy Engine for Real-Time Mid-Price Forecasting in HFT

**arXiv**: 2412.19372  
**Authors**: Adamantios Ntakaris et al.  
**Date**: December 2024 (v2: December 2024)

### 1. Core Information
- **Research Question**: Can reinforcement learning-based adaptive learning improve real-time mid-price forecasting in high-frequency trading?
- **Key Contributions**: 
  - Introduces Adaptive Learning Policy Engine (ALPE) - RL-based agent for batch-free forecasting
  - Incorporates adaptive epsilon decay for exploration-exploitation balance
  - Outperforms diverse ML/DL models on NASDAQ LOB data

### 2. Architecture Details
- **Model Type**: Reinforcement Learning (RL) with RBFNN foundation
- **Input Features**: Level 1 LOB data (bid/ask prices, volumes)
- **Output**: Mid-price direction/movement prediction
- **Architecture**: 
  - Builds on Radial Basis Function Neural Networks (RBFNN)
  - RL agent with adaptive epsilon decay
  - Automated feature importance via MDI and gradient descent
- **Training**: Batch-free, immediate forecasting with RL optimization

### 3. Data & Experiments
- **Dataset**: NASDAQ Level 1 LOB, 100 S&P 500 stocks
- **Time Period**: September-November 2022
- **Baselines**: ML models, DL models, previous RBFNN work
- **Metrics**: Forecasting accuracy (specific metrics not detailed in abstract)
- **Results**: ALPE outperforms diverse ML/DL models

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: RL-based adaptive learning for HFT without batch processing
- **Best Use Case**: Real-time mid-price forecasting in liquid markets
- **Computational Requirements**: Moderate (RBFNN-based)
- **Production Readiness**: Medium - requires RL tuning expertise

---

## Paper 2: Deep Learning for VWAP Execution in Crypto Markets: Beyond the Volume Curve

**arXiv**: 2502.13722  
**Authors**: Remi Genet  
**Date**: February 2025 (v2: April 2025)

### 1. Core Information
- **Research Question**: Can direct optimization of VWAP execution objective outperform traditional volume curve prediction approaches?
- **Key Contributions**:
  - Deep learning framework that directly optimizes VWAP execution
  - Bypasses intermediate volume curve prediction step
  - Uses automatic differentiation and custom loss functions
  - Demonstrates lower VWAP slippage than conventional methods

### 2. Architecture Details
- **Model Type**: Deep Learning (direct optimization)
- **Input Features**: Market data (prices, volumes)
- **Output**: Order allocation for VWAP execution
- **Architecture**: 
  - Direct optimization via custom loss functions
  - Even naive linear models show improvement when directly optimized
- **Training**: End-to-end optimization of VWAP slippage objective

### 3. Data & Experiments
- **Dataset**: Cryptocurrency markets
- **Time Period**: Not specified
- **Baselines**: Conventional volume curve prediction methods
- **Metrics**: VWAP slippage
- **Results**: Direct optimization achieves consistently lower slippage

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Automatic differentiation framework implied

### 5. Key Insights
- **Innovation**: Direct objective optimization vs. intermediate prediction
- **Best Use Case**: VWAP execution in volatile markets (crypto, equities)
- **Computational Requirements**: Low to moderate
- **Production Readiness**: High - principle applicable across asset classes

---

## Paper 3: VWAP Execution with Signature-Enhanced Transformers: A Multi-Asset Learning Approach

**arXiv**: 2503.02680  
**Authors**: Remi Genet  
**Date**: March 2025

### 1. Core Information
- **Research Question**: Can a single neural network trained across multiple assets outperform asset-specific models for VWAP execution?
- **Key Contributions**:
  - Globally-fitted model with signature features (GFT-Sig)
  - Combines Transformer architecture with path signatures
  - Demonstrates cross-asset generalization
  - Superior performance on both absolute and quadratic VWAP loss

### 2. Architecture Details
- **Model Type**: Transformer + Path Signatures
- **Input Features**: Price-volume trajectories, signature features
- **Output**: Order allocation for VWAP execution
- **Architecture**:
  - Transformer-based design
  - Path signatures for geometric feature extraction
  - Global parameter sharing across assets
- **Training**: Multi-asset training on 80 cryptocurrency pairs

### 3. Data & Experiments
- **Dataset**: 80 cryptocurrency trading pairs
- **Time Period**: Hourly data (specific period not stated)
- **Baselines**: Asset-specific models
- **Metrics**: Absolute VWAP loss, Quadratic VWAP loss
- **Results**: GFT-Sig outperforms asset-specific approaches; generalizes to out-of-sample assets

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Path signatures library implied

### 5. Key Insights
- **Innovation**: Multi-asset learning with signature-enhanced Transformers
- **Best Use Case**: Multi-asset VWAP execution, especially crypto
- **Computational Requirements**: Moderate (Transformer-based)
- **Production Readiness**: Medium-High - scalable approach

---

## Paper 4: Technical Indicator Networks (TINs): An Interpretable Neural Architecture Modernizing Classical Technical Analysis

**arXiv**: 2507.20202  
**Authors**: Longfei Lu  
**Date**: July 2025 (v2: December 2025)

### 1. Core Information
- **Research Question**: Can classical technical indicators be reformulated as trainable, interpretable neural modules?
- **Key Contributions**:
  - Introduces Technical Indicator Networks (TINs)
  - Reformulates rule-based financial heuristics as trainable modules
  - Preserves mathematical definitions while enabling optimization
  - Validates on DJIA constituents with MACD TIN

### 2. Architecture Details
- **Model Type**: Interpretable Neural Network
- **Input Features**: Price data, technical indicator inputs
- **Output**: Trading signals
- **Architecture**:
  - Vectorized layer operators for averaging, clipping, ratio computation
  - Transparent network construction
  - Principled initialization from classical indicators
  - Supports reinforcement learning
- **Training**: Diverse learning paradigms including RL

### 3. Data & Experiments
- **Dataset**: Dow Jones Industrial Average constituents
- **Time Period**: Not specified
- **Baselines**: Traditional indicator-based strategies
- **Metrics**: Risk-adjusted performance
- **Results**: Improved risk-adjusted performance vs. traditional strategies

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Interpretable neural architecture from classical indicators
- **Best Use Case**: Algorithmic trading with interpretability requirements
- **Computational Requirements**: Low-Moderate
- **Production Readiness**: High - commercial potential noted

---

## Paper 5: Advanced Ensemble Deep Learning Framework for Stock Price Prediction (VAE + Transformer + LSTM)

**arXiv**: 2503.22192  
**Authors**: Anindya Sarkar  
**Date**: March 2025

### 1. Core Information
- **Research Question**: Can ensemble of VAE, Transformer, and LSTM achieve superior stock price prediction?
- **Key Contributions**:
  - Ensemble framework combining VAE, Transformer, LSTM
  - Captures both linear and non-linear relations
  - Scales predictors based on market conditions
  - Consistently high accuracy across datasets

### 2. Architecture Details
- **Model Type**: Ensemble (VAE + Transformer + LSTM)
- **Input Features**: Technical indicators, price data
- **Output**: Stock price prediction (directional)
- **Architecture**:
  - VAE: Learns latent representations from high-dimensional data
  - Transformer: Recognizes long-term patterns
  - LSTM: Captures temporal dynamics and fluctuations
  - Ensemble combination mechanism
- **Training**: Ensemble training with market-condition scaling

### 3. Data & Experiments
- **Dataset**: Multiple stock datasets
- **Time Period**: Not specified
- **Baselines**: Single models, conventional forecasting
- **Metrics**: Directional accuracy, prediction disparity
- **Results**: Exceptional directional performance, small prediction disparity

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Three-model ensemble for complementary strengths
- **Best Use Case**: Stock price prediction, algorithmic trading
- **Computational Requirements**: High (three deep models)
- **Production Readiness**: Medium - ensemble complexity

---

## Paper 6: Comparative Analysis of LSTM, GRU, and Transformer Models for Stock Price Prediction

**arXiv**: 2411.05790  
**Authors**: Jue Xiao  
**Date**: October 2024

### 1. Core Information
- **Research Question**: How do LSTM, GRU, and Transformer models compare for stock price trend prediction?
- **Key Contributions**:
  - Comprehensive comparison of three architectures
  - Tesla stock data 2015-2024
  - LSTM achieves 94% accuracy

### 2. Architecture Details
- **Model Type**: LSTM, GRU, Transformer (comparative)
- **Input Features**: Historical price data
- **Output**: Stock trend prediction
- **Architecture**:
  - LSTM: Standard long short-term memory
  - GRU: Gated recurrent unit
  - Transformer: Attention-based architecture
- **Training**: Model training on Tesla data

### 3. Data & Experiments
- **Dataset**: Tesla (TSLA) stock
- **Time Period**: 2015-2024
- **Baselines**: LSTM, GRU, Transformer comparison
- **Metrics**: Accuracy
- **Results**: LSTM achieves 94% accuracy (best among compared)

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Direct architecture comparison on same dataset
- **Best Use Case**: Stock trend prediction
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium - single-stock focus limits generalization

---

## Paper 7: Deep Limit Order Book Forecasting: A Microstructural Guide

**arXiv**: 2403.09267  
**Authors**: Antonio Briola et al.  
**Date**: March 2024 (v4: June 2024)

### 1. Core Information
- **Research Question**: How effective are deep learning methods for LOB mid-price change forecasting, and do high forecasting metrics translate to actionable signals?
- **Key Contributions**:
  - Releases LOBFrame open-source codebase
  - Shows microstructural characteristics influence DL efficacy
  - Proposes operational framework for practical evaluation
  - Traditional ML metrics inadequate for LOB context

### 2. Architecture Details
- **Model Type**: Deep Learning (various SOTA models)
- **Input Features**: LOB data (multiple levels)
- **Output**: Mid-price change predictions
- **Architecture**: Multiple SOTA deep learning models evaluated
- **Training**: Standard DL training on LOB data

### 3. Data & Experiments
- **Dataset**: NASDAQ stocks (heterogeneous set)
- **Time Period**: Not specified
- **Baselines**: Various DL models
- **Metrics**: Traditional ML metrics + proposed operational framework
- **Results**: High forecasting power doesn't necessarily mean actionable signals

### 4. Code
- **GitHub**: LOBFrame (open-source codebase released)
- **Framework**: Not specified
- **Libraries**: LOBFrame

### 5. Key Insights
- **Innovation**: Operational framework for practical LOB forecast evaluation
- **Best Use Case**: LOB forecasting with practical trading focus
- **Computational Requirements**: Varies by model
- **Production Readiness**: High - open-source tooling available

---

## Paper 8: Deep Learning Meets Queue-Reactive: Framework for Realistic LOB Simulation

**arXiv**: 2501.08822  
**Authors**: Hamza Bodor et al.  
**Date**: January 2025

### 1. Core Information
- **Research Question**: Can deep learning enhance the Queue-Reactive model for realistic LOB simulation?
- **Key Contributions**:
  - Multidimensional Deep Queue-Reactive (MDQR) model
  - Relaxes queue independence assumption
  - Enriches state space with market features
  - Models order size distributions

### 2. Architecture Details
- **Model Type**: Neural Network + Point Process
- **Input Features**: Market features, queue states
- **Output**: Order event simulation
- **Architecture**:
  - Neural network for complex dependencies
  - Point-process foundation (interpretable)
  - Cross-queue correlation modeling
- **Training**: Learning from Bund futures market data

### 3. Data & Experiments
- **Dataset**: Bund futures market
- **Time Period**: Not specified
- **Baselines**: Original Queue-Reactive model
- **Metrics**: Square-root law, cross-queue correlations, order size patterns
- **Results**: Captures key market properties, maintains computational efficiency

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Deep learning enhancement of Queue-Reactive framework
- **Best Use Case**: LOB simulation for RL backtesting, strategy development
- **Computational Requirements**: Moderate (efficient for practical use)
- **Production Readiness**: High - designed for practical applications

---

## Paper 9: ClusterLOB: Enhancing Trading Strategies by Clustering Orders in LOBs

**arXiv**: 2504.20349  
**Authors**: Yichi Zhang et al.  
**Date**: April 2025 (v3: May 2025)

### 1. Core Information
- **Research Question**: Can clustering market events in LOB data improve trading strategy performance?
- **Key Contributions**:
  - ClusterLOB method for clustering market-by-order (MBO) data
  - Identifies directional, opportunistic, and market-making participants
  - Order flow imbalance signals from clusters
  - Superior Sharpe ratio vs. benchmarks

### 2. Architecture Details
- **Model Type**: K-means++ Clustering
- **Input Features**: Market events with 6 time-dependent features
- **Output**: Cluster assignments (3 clusters)
- **Architecture**:
  - Feature augmentation for market events
  - K-means++ clustering
  - Order flow imbalance computation per cluster
- **Training**: Unsupervised clustering

### 3. Data & Experiments
- **Dataset**: NASDAQ MBO data (small, medium, large-tick stocks)
- **Time Period**: One year
- **Baselines**: Benchmark trading strategies without clustering
- **Metrics**: Sharpe ratio
- **Results**: Best clustering strategy outperforms benchmarks in test dataset

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: K-means++ (standard)

### 5. Key Insights
- **Innovation**: Participant behavior clustering for trading signals
- **Best Use Case**: LOB-based trading strategy enhancement
- **Computational Requirements**: Low (clustering is efficient)
- **Production Readiness**: High - practical framework

---

## Paper 10: Event-Based LOB Simulation under Neural Hawkes Process: Application in Market-Making

**arXiv**: 2502.17417  
**Authors**: Luca Lalor et al.  
**Date**: February 2025

### 1. Core Information
- **Research Question**: Can Neural Hawkes processes capture LOB event dynamics for realistic simulation?
- **Key Contributions**:
  - Event-driven LOB model with 12 event types
  - Neural Hawkes process with LSTM
  - Captures long/short-term event interactions
  - Applied to Deep RL Market-Making framework

### 2. Architecture Details
- **Model Type**: Neural Hawkes Process + LSTM
- **Input Features**: LOB event types and timestamps
- **Output**: Simulated event sequences, midprice process
- **Architecture**:
  - Neural Hawkes process (state-of-the-art)
  - LSTM for dynamic relationships
  - 12 LOB event types modeled
- **Training**: Learning from historical event data

### 3. Data & Experiments
- **Dataset**: Exchange-based financial markets
- **Time Period**: Not specified
- **Baselines**: Traditional Hawkes process models
- **Metrics**: Price volatility characteristics, trade order fill distribution
- **Results**: Captures broader price fluctuation characteristics; sim aligns with real data

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Neural Hawkes for multi-event LOB simulation
- **Best Use Case**: Market-making simulation, RL training
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium - simulation quality validated

---

## Paper 11: DiffVolume: Diffusion Models for Volume Generation in LOBs

**arXiv**: 2508.08698  
**Authors**: Zhuohan Wang et al.  
**Date**: August 2025

### 1. Core Information
- **Research Question**: Can diffusion models generate realistic high-dimensional LOB volume snapshots?
- **Key Contributions**:
  - DiffVolume: Conditional diffusion model for LOB volume
  - Evaluates realism, counterfactual generation, downstream prediction
  - Better reproduces statistical properties than GANs
  - Synthetic data improves liquidity forecasting

### 2. Architecture Details
- **Model Type**: Conditional Diffusion Model
- **Input Features**: Past volume history, time of day, target liquidity profile (optional)
- **Output**: Future LOB volume snapshots
- **Architecture**:
  - Conditional diffusion process
  - Temporal and liquidity-dependent pattern modeling
- **Training**: Diffusion model training on volume data

### 3. Data & Experiments
- **Dataset**: LOB volume data
- **Time Period**: Not specified
- **Baselines**: GAN-based approaches
- **Metrics**: Marginal distribution, spatial correlation, autocorrelation decay, downstream forecast performance
- **Results**: Better statistical property reproduction; synthetic data improves forecasting

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Diffusion model libraries implied

### 5. Key Insights
- **Innovation**: First application of diffusion models to LOB volume generation
- **Best Use Case**: LOB simulation, data augmentation, counterfactual analysis
- **Computational Requirements**: High (diffusion models)
- **Production Readiness**: Medium - promising but computationally intensive

---

## Paper 12: DeepTraderX: Challenging Conventional Trading Strategies with Deep Learning

**arXiv**: 2403.18831  
**Authors**: Armand Cismaru et al.  
**Date**: March 2024

### 1. Core Information
- **Research Question**: Can a simple deep learning trader compete with conventional strategies in multi-threaded market simulation?
- **Key Contributions**:
  - DeepTraderX (DTX): Simple DL-based trader
  - Learns by watching other strategies' prices
  - Maps market state to bid/ask quotes
  - Rivals/surpasses public-domain traders

### 2. Architecture Details
- **Model Type**: Deep Learning (simple architecture)
- **Input Features**: Level-2 LOB data, market state
- **Output**: Bid/ask order prices
- **Architecture**:
  - Simple DL model (architecture not detailed)
  - Processes market state S at timestep T
  - Price P determination for market orders
- **Training**: Learning from historical Level-2 data, watching other strategies

### 3. Data & Experiments
- **Dataset**: Historical Level-2 LOB data
- **Time Period**: ~500 simulated market days
- **Baselines**: Best strategies in literature, public-domain traders
- **Metrics**: Performance comparison, statistical analysis
- **Results**: DTX rivals and often surpasses conventional strategies

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Simple DL model effective in multi-threaded simulation
- **Best Use Case**: Multi-threaded market simulation, market-making
- **Computational Requirements**: Low-Moderate (simple model)
- **Production Readiness**: Medium - simulation-validated

---

# SECTION 2: GNN & Advanced Architectures (8 Papers)

---

## Paper 13: Trading Graph Neural Network (TGNN)

**arXiv**: 2504.07923  
**Authors**: Xian Wu  
**Date**: April 2025

### 1. Core Information
- **Research Question**: Can GNNs structurally estimate the impact of asset, dealer, and relationship features on asset prices in trading networks?
- **Key Contributions**:
  - Trading Graph Neural Network (TGNN) algorithm
  - Combines Simulated Method of Moments (SMM) with GNN
  - Outperforms reduced-form methods with network centrality
  - Works on networks with any structure

### 2. Architecture Details
- **Model Type**: Graph Neural Network + SMM
- **Input Features**: Asset features, dealer features, relationship features
- **Output**: Asset price impact estimates
- **Architecture**:
  - GNN for network structure
  - SMM for parameter estimation
  - Handles trader and asset heterogeneity
- **Training**: SMM-GNN combined training

### 3. Data & Experiments
- **Dataset**: Trading networks
- **Time Period**: Not specified
- **Baselines**: Reduced-form methods with network centrality
- **Metrics**: Prediction accuracy
- **Results**: Outperforms existing methods

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: GNN libraries implied

### 5. Key Insights
- **Innovation**: SMM-GNN hybrid for trading networks
- **Best Use Case**: Trading network analysis, dealer-asset relationships
- **Computational Requirements**: Moderate-High (GNN)
- **Production Readiness**: Medium

---

## Paper 14: Stock Price Prediction Using Hybrid LSTM-GNN Model

**arXiv**: 2502.15813  
**Authors**: Atta Badii et al.  
**Date**: February 2025

### 1. Core Information
- **Research Question**: Can hybrid LSTM-GNN model improve stock price prediction by integrating time-series and graph-based analysis?
- **Key Contributions**:
  - Hybrid LSTM-GNN architecture
  - LSTM for temporal patterns
  - GNN for inter-stock relationships (Pearson correlation)
  - 10.6% MSE reduction vs. standalone LSTM

### 2. Architecture Details
- **Model Type**: LSTM + GNN Hybrid
- **Input Features**: Stock price time series, inter-stock correlations
- **Output**: Stock price prediction
- **Architecture**:
  - LSTM: Temporal pattern capture
  - GNN: Polyadic dependencies via Pearson correlation
  - Expanding window validation
- **Training**: Expanding window approach for continuous learning

### 3. Data & Experiments
- **Dataset**: Historical stock data
- **Time Period**: Not specified
- **Baselines**: LSTM, linear regression, CNN, dense networks
- **Metrics**: MSE
- **Results**: MSE 0.00144 vs. LSTM 0.00161 (10.6% improvement)

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Temporal + relational data integration
- **Best Use Case**: Multi-stock price prediction
- **Computational Requirements**: Moderate-High
- **Production Readiness**: Medium

---

## Paper 15: Mamba Meets Financial Markets: Graph-Mamba for Stock Price Prediction

**arXiv**: 2410.03707  
**Authors**: Ali Mehrabian  
**Date**: October 2024 (v2: January 2025)

### 1. Core Information
- **Research Question**: Can Mamba architecture with GNN achieve better accuracy with lower complexity than Transformers for stock prediction?
- **Key Contributions**:
  - SAMBA framework (Stock + Mamba)
  - Bidirectional Mamba block for long-term dependencies
  - Adaptive graph convolution for stock feature dependencies
  - Near-linear computational complexity

### 2. Architecture Details
- **Model Type**: Mamba + GNN
- **Input Features**: Historical price data, daily stock features
- **Output**: Stock return prediction
- **Architecture**:
  - Bidirectional Mamba block
  - Adaptive graph convolution
  - Near-linear complexity
- **Training**: Standard deep learning training

### 3. Data & Experiments
- **Dataset**: Stock data
- **Time Period**: Not specified
- **Baselines**: Transformer, LSTM, CNN, SOTA models
- **Metrics**: Prediction accuracy, computational complexity
- **Results**: Significantly outperforms baselines with low complexity

### 4. Code
- **GitHub**: https://github.com/Ali-Meh619/SAMBA
- **Framework**: Not specified (likely PyTorch)
- **Libraries**: Mamba, GNN libraries

### 5. Key Insights
- **Innovation**: Mamba architecture for finance (first application)
- **Best Use Case**: Real-time trading, long-sequence processing
- **Computational Requirements**: Low (near-linear complexity)
- **Production Readiness**: High - code available, efficient

---

## Paper 16: TLOB: Transformer with Dual Attention for Price Trend Prediction with LOB Data

**arXiv**: 2502.15757  
**Authors**: Leonardo Berti et al.  
**Date**: February 2025 (v3: May 2025)

### 1. Core Information
- **Research Question**: Can a Transformer with dual attention mechanism improve price trend prediction generalization across market conditions?
- **Key Contributions**:
  - TLOB: Transformer with dual attention (spatial + temporal)
  - New labeling method removing horizon bias
  - Outperforms SOTA across 4 horizons, 3 datasets
  - Shows declining stock predictability over time (-6.68 F1)

### 2. Architecture Details
- **Model Type**: Transformer (dual attention)
- **Input Features**: LOB data
- **Output**: Price trend prediction
- **Architecture**:
  - Dual attention: spatial + temporal
  - Adaptive focus on market microstructure
  - Effective for longer-horizon predictions
- **Training**: Standard Transformer training

### 3. Data & Experiments
- **Dataset**: FI-2010, NASDAQ, Bitcoin
- **Time Period**: Not specified
- **Baselines**: SOTA methods, simple MLP
- **Metrics**: F1-score
- **Results**: Outperforms SOTA on every dataset and horizon

### 4. Code
- **GitHub**: https://github.com/LeonardoBerti00/TLOB
- **Framework**: Not specified (likely PyTorch)
- **Libraries**: Transformer libraries

### 5. Key Insights
- **Innovation**: Dual attention for LOB data
- **Best Use Case**: Price trend prediction across assets
- **Computational Requirements**: Moderate (Transformer)
- **Production Readiness**: High - code available

---

## Paper 17: HAELT: Hybrid Attentive Ensemble Learning Transformer

**arXiv**: 2506.13981  
**Authors**: Dan Bui  
**Date**: June 2025

### 1. Core Information
- **Research Question**: Can hybrid ensemble of ResNet, LSTM, and Transformer improve high-frequency stock prediction?
- **Key Contributions**:
  - HAELT framework
  - ResNet-based noise mitigation
  - Temporal self-attention
  - Hybrid LSTM-Transformer core
  - Adaptive ensemble based on recent performance

### 2. Architecture Details
- **Model Type**: Ensemble (ResNet + LSTM + Transformer)
- **Input Features**: High-frequency stock data
- **Output**: Price movement direction (up/down)
- **Architecture**:
  - ResNet: Noise mitigation
  - Temporal self-attention: Dynamic history focus
  - LSTM-Transformer: Local + long-range dependencies
  - Adaptive ensemble weighting
- **Training**: Performance-based adaptive ensemble

### 3. Data & Experiments
- **Dataset**: Apple Inc. (AAPL) hourly data
- **Time Period**: January 2024 - May 2025
- **Baselines**: Not specified
- **Metrics**: F1-Score
- **Results**: Highest F1-Score on test set

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Adaptive ensemble of multiple architectures
- **Best Use Case**: High-frequency stock prediction
- **Computational Requirements**: High (ensemble)
- **Production Readiness**: Medium

---

## Paper 18: CITRAS: Covariate-Informed Transformer for Time Series Forecasting

**arXiv**: 2503.24007  
**Authors**: Yosuke Yamaguchi et al.  
**Date**: March 2025 (v3: August 2025)

### 1. Core Information
- **Research Question**: Can decoder-only Transformer effectively leverage future covariates for time series forecasting?
- **Key Contributions**:
  - CITRAS: Covariate-Informed Transformer
  - KV Shift for future covariate incorporation
  - Attention Score Smoothing for global dependencies
  - Outperforms SOTA on 13 benchmarks

### 2. Architecture Details
- **Model Type**: Decoder-only Transformer
- **Input Features**: Multiple targets, past covariates, future covariates
- **Output**: Time series forecast
- **Architecture**:
  - Patch-wise cross-variate attention
  - KV Shift: Future covariate integration
  - Attention Score Smoothing: Local to global dependencies
- **Training**: Standard Transformer training

### 3. Data & Experiments
- **Dataset**: 13 real-world benchmarks
- **Time Period**: Various
- **Baselines**: SOTA time series models
- **Metrics**: Forecasting accuracy (various)
- **Results**: Outperforms SOTA on all benchmarks

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Future covariate handling in Transformer
- **Best Use Case**: Time series forecasting with covariates
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium

---

## Paper 19: Bridging Short- and Long-Term Dependencies: CNN-Transformer Hybrid

**arXiv**: 2504.19309  
**Authors**: Tiantian Tu  
**Date**: April 2025

### 1. Core Information
- **Research Question**: Can CNN-Transformer hybrid effectively model both short- and long-term dependencies in financial time series?
- **Key Contributions**:
  - Hybrid CNN-Transformer architecture
  - CNN for localized short-term patterns
  - Transformer for global temporal relationships
  - Outperforms traditional and DL methods on S&P 500

### 2. Architecture Details
- **Model Type**: CNN + Transformer Hybrid
- **Input Features**: Stock price time series
- **Output**: Intraday stock price forecast
- **Architecture**:
  - CNN layers: Short-term pattern extraction
  - Transformer layers: Long-term dependency modeling
  - Combined architecture
- **Training**: Standard deep learning training

### 3. Data & Experiments
- **Dataset**: S&P 500 constituents
- **Time Period**: Not specified
- **Baselines**: Traditional statistical models, popular DL methods
- **Metrics**: Forecasting accuracy
- **Results**: Outperforms baselines

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: CNN-Transformer hybrid for finance
- **Best Use Case**: Intraday stock price forecasting
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium

---

## Paper 20: Transformer Based Time-Series Forecasting for Stock (Stockformer)

**arXiv**: 2502.09625  
**Authors**: Shuozhe Li  
**Date**: January 2025

### 1. Core Information
- **Research Question**: Can mutated Transformer (Stockformer) improve multivariate stock forecasting vs. naive autoregression?
- **Key Contributions**:
  - Stockformer: Mutated Transformer variant
  - Multivariate forecasting formulation
  - Attention mechanism for multivariate analysis

### 2. Architecture Details
- **Model Type**: Transformer (mutated)
- **Input Features**: Multivariate stock data
- **Output**: Stock forecast
- **Architecture**:
  - Modified Transformer architecture
  - Attention for multivariate relationships
- **Training**: Multivariate forecasting training

### 3. Data & Experiments
- **Dataset**: Stock data
- **Time Period**: Not specified
- **Baselines**: Naive autoregression
- **Metrics**: Not specified in abstract
- **Results**: Multivariate approach superior

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Multivariate Transformer for stocks
- **Best Use Case**: Multivariate stock forecasting
- **Computational Requirements**: Moderate
- **Production Readiness**: Low-Medium

---

# SECTION 3: Volatility & Statistical Arbitrage (9 Papers)

---

## Paper 21: Unified GARCH-Recurrent Neural Network in Financial Volatility Forecasting

**arXiv**: 2504.09380  
**Authors**: Jingyi Wei et al.  
**Date**: April 2025 (v2: November 2025)

### 1. Core Information
- **Research Question**: Can embedding GARCH dynamics within RNNs improve volatility forecasting accuracy and interpretability?
- **Key Contributions**:
  - GARCH-GRU and GARCH-LSTM architectures
  - GARCH(1,1) integrated into gating structure
  - Preserves economically meaningful GARCH parameters
  - Outperforms classical GARCH, hybrids, and Transformer

### 2. Architecture Details
- **Model Type**: GARCH-RNN Hybrid (GARCH-GRU, GARCH-LSTM)
- **Input Features**: Financial time series returns
- **Output**: Volatility forecast
- **Architecture**:
  - GARCH(1,1) volatility update in multiplicative gating
  - GRU/LSTM for nonlinear temporal dependencies
  - Interpretable GARCH parameters
- **Training**: Hybrid econometric-DL training

### 3. Data & Experiments
- **Dataset**: Major U.S. equity indices
- **Time Period**: Includes COVID-19 stress period
- **Baselines**: Classical GARCH, pipeline hybrids, Transformer
- **Metrics**: MSE, MAE, SMAPE, R², VaR violation ratios, Pinball loss
- **Results**: GARCH-GRU best accuracy-efficiency tradeoff; superior during COVID

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Direct GARCH integration into RNN gating
- **Best Use Case**: Volatility forecasting, risk management
- **Computational Requirements**: Moderate (GARCH-GRU 3x faster than GARCH-LSTM)
- **Production Readiness**: High - interpretable, robust

---

## Paper 22: Deep Learning Enhanced Multivariate GARCH (LSTM-BEKK)

**arXiv**: 2506.02796  
**Authors**: Chen Liu et al.  
**Date**: June 2025

### 1. Core Information
- **Research Question**: Can LSTM-enhanced BEKK model improve multivariate volatility forecasting?
- **Key Contributions**:
  - LSTM-BEKK framework
  - Combines RNN flexibility with BEKK structure
  - Captures nonlinear, dynamic, high-dimensional dependencies
  - Superior portfolio risk forecast

### 2. Architecture Details
- **Model Type**: LSTM + BEKK Hybrid
- **Input Features**: Multivariate financial returns
- **Output**: Multivariate volatility forecast, covariance matrix
- **Architecture**:
  - LSTM for nonlinear dynamics
  - BEKK for multivariate GARCH structure
  - Interpretable BEKK parameters
- **Training**: Hybrid econometric-DL training

### 3. Data & Experiments
- **Dataset**: Multiple equity markets
- **Time Period**: Not specified
- **Baselines**: Traditional multivariate GARCH
- **Metrics**: Out-of-sample portfolio risk forecast
- **Results**: Superior performance, maintains interpretability

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: LSTM-BEKK for multivariate volatility
- **Best Use Case**: Portfolio risk management, multivariate volatility
- **Computational Requirements**: Moderate-High
- **Production Readiness**: Medium-High

---

## Paper 23: Statistical Arbitrage in Polish Equities Using Deep Learning

**arXiv**: 2512.02037  
**Authors**: Marek Adamczyk  
**Date**: November 2025

### 1. Core Information
- **Research Question**: Can LSTM-based replication improve pairs trading in Polish equities market?
- **Key Contributions**:
  - LSTM-based asset replication for pairs trading
  - Adapts Avellaneda-Lee (2008) to Polish market
  - Compares PCA, ETF, LSTM replication methods
  - PCA: 20% cumulative return, Sharpe 2.63 (2017-2019)

### 2. Architecture Details
- **Model Type**: LSTM (for replication)
- **Input Features**: Stock returns, risk factors
- **Output**: Asset replication, residual for mean reversion
- **Architecture**:
  - LSTM for factor representation
  - PCA, ETF alternatives
  - Ornstein-Uhlenbeck process for residuals
- **Training**: LSTM training for replication

### 3. Data & Experiments
- **Dataset**: WIG20, mWIG40, sector indices (Polish market)
- **Time Period**: 2017-2019, 2020
- **Baselines**: PCA, ETF replication
- **Metrics**: Cumulative return, Sharpe ratio
- **Results**: PCA best 2017-2019; only ETF profitable in 2020; LSTM negative but promising

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: LSTM for pairs trading replication
- **Best Use Case**: Statistical arbitrage in emerging markets
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium - LSTM needs optimization

---

## Paper 24: Attention Factors for Statistical Arbitrage

**arXiv**: 2510.11616  
**Authors**: Elliot L. Epstein  
**Date**: October 2025

### 1. Core Information
- **Research Question**: Can attention-based latent factors improve statistical arbitrage profitability?
- **Key Contributions**:
  - Attention Factors: Conditional latent factors for arbitrage
  - Firm characteristic embeddings
  - Joint factor identification and trading policy
  - Sharpe ratio 4+ gross, 2.3 net (24 years, US equities)

### 2. Architecture Details
- **Model Type**: Attention-based factor model + sequence model
- **Input Features**: Firm characteristics
- **Output**: Trading signals, factor exposures
- **Architecture**:
  - Characteristic embeddings
  - Attention for factor learning
  - Sequence model for time-series signals
- **Training**: Joint optimization of factors and trading policy

### 3. Data & Experiments
- **Dataset**: Largest U.S. equities
- **Time Period**: 24 years
- **Baselines**: Traditional factor models
- **Metrics**: Sharpe ratio (gross and net)
- **Results**: Sharpe 4+ gross, 2.3 net of transaction costs

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Attention-based factors for stat arb
- **Best Use Case**: Statistical arbitrage, factor investing
- **Computational Requirements**: Moderate-High
- **Production Readiness**: High - exceptional results

---

## Paper 25: Hybrid Ridgelet Deep Neural Networks for Data-Driven Arbitrage

**arXiv**: 2510.10599  
**Authors**: Sanjay Mohanty  
**Date**: October 2025

### 1. Core Information
- **Research Question**: Can Ridgelet Transform enhance deep neural networks for arbitrage detection?
- **Key Contributions**:
  - Hybrid Ridgelet-DNN framework
  - Ridgelet Transform for high-dimensional sparse structures
  - Scalable to 50 assets
  - Robust during market volatility

### 2. Architecture Details
- **Model Type**: Ridgelet Transform + Deep Neural Network
- **Input Features**: Multi-asset market data
- **Output**: Arbitrage detection, trading strategies
- **Architecture**:
  - Ridgelet Transform on Borel measurable functions
  - DNN for strategy optimization
  - Activation function engineering for stability
- **Training**: Optimization on HPC cluster

### 3. Data & Experiments
- **Dataset**: Multi-asset data (up to 50 assets)
- **Time Period**: Not specified
- **Baselines**: Not specified
- **Metrics**: Profitability
- **Results**: Strong profitability, robust during volatility

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Ridgelet Transform for arbitrage
- **Best Use Case**: Multi-asset arbitrage detection
- **Computational Requirements**: High (HPC cluster used)
- **Production Readiness**: Low-Medium

---

## Paper 26: Statistical Arbitrage via Graph Clustering in US Equities

**arXiv**: 2406.10695  
**Authors**: Robert Ślepaczuk  
**Date**: June 2024

### 1. Core Information
- **Research Question**: Can graph clustering algorithms improve multi-pair statistical arbitrage?
- **Key Contributions**:
  - Graph clustering for pair selection
  - Kelly criterion integration
  - Ensemble of ML classifiers
  - Optimized take profit/stop loss functions

### 2. Architecture Details
- **Model Type**: Graph Clustering + Ensemble ML
- **Input Features**: Stock data, relationships
- **Output**: Trading signals, pair selections
- **Architecture**:
  - Graph clustering for pair identification
  - Ensemble ML classifiers
  - Kelly criterion for position sizing
- **Training**: Graph clustering + ML ensemble training

### 3. Data & Experiments
- **Dataset**: US equities
- **Time Period**: Not specified
- **Baselines**: Appropriate benchmarks
- **Metrics**: Risk-adjusted returns, transaction cost immunity
- **Results**: All approaches outperformed benchmarks

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Graph clustering for stat arb pair selection
- **Best Use Case**: Multi-pair statistical arbitrage
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium-High

---

## Paper 27: Framework for Predictive Directional Trading Based on Volatility and Causal Inference

**arXiv**: 2507.09347  
**Authors**: Ivan Letteri  
**Date**: July 2025

### 1. Core Information
- **Research Question**: Can volatility-based clustering combined with causal inference identify profitable lead-lag relationships?
- **Key Contributions**:
  - GMM for volatility-based stock clustering
  - Multi-stage causal inference (GCT, PCMCI, ETE)
  - DTW + KNN for optimal lag determination
  - 15.38% return vs. 10.39% buy-and-hold

### 2. Architecture Details
- **Model Type**: GMM + Causal Inference + DTW + KNN
- **Input Features**: Stock returns, volatility
- **Output**: Trading signals, pair selections
- **Architecture**:
  - GMM: Volatility clustering
  - GCT, PCMCI, ETE: Causal relationship identification
  - DTW + KNN: Optimal lag determination
- **Training**: Causal inference pipeline

### 3. Data & Experiments
- **Dataset**: 9 prominent stocks
- **Time Period**: 3-year clustering, June-August 2023 backtest
- **Baselines**: Buy-and-Hold
- **Metrics**: Total return, Sharpe Ratio, win rate
- **Results**: 15.38% return, Sharpe up to 2.17, win rate up to 100%

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Volatility + causal inference for directional trading
- **Best Use Case**: Pairs trading, lead-lag strategies
- **Computational Requirements**: Moderate
- **Production Readiness**: Medium

---

## Paper 28: FinCARE: Financial Causal Analysis with Reasoning and Evidence

**arXiv**: 2510.20221  
**Authors**: Bhaskarjit Sarmah et al.  
**Date**: October 2025

### 1. Core Information
- **Research Question**: Can knowledge graph + LLM reasoning enhance causal discovery for portfolio management?
- **Key Contributions**:
  - Hybrid framework: causal discovery + KG + LLM
  - Enhances PC, GES, NOTEARS algorithms
  - KG constraints from SEC 10-K filings
  - F1 improvements: PC +36%, GES +100%, NOTEARS +366%

### 2. Architecture Details
- **Model Type**: Causal Discovery + Knowledge Graph + LLM
- **Input Features**: Financial variables, 10-K filings
- **Output**: Causal graph, counterfactual predictions
- **Architecture**:
  - PC, GES, NOTEARS: Causal discovery algorithms
  - Knowledge graph constraints
  - LLM for hypothesis generation
- **Training**: Hybrid training with KG constraints

### 3. Data & Experiments
- **Dataset**: Synthetic financial dataset (500 firms, 18 variables)
- **Time Period**: Not applicable (synthetic)
- **Baselines**: Standard PC, GES, NOTEARS
- **Metrics**: F1 score, MAE for counterfactuals, directional accuracy
- **Results**: Major F1 improvements; MAE 0.003610; perfect directional accuracy

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Causal discovery libraries, LLM

### 5. Key Insights
- **Innovation**: KG + LLM enhancement of causal discovery
- **Best Use Case**: Portfolio risk management, scenario analysis
- **Computational Requirements**: High (LLM integration)
- **Production Readiness**: Medium

---

## Paper 29: Towards Causal Market Simulators

**arXiv**: 2511.04469  
**Authors**: Dennis Thumm et al.  
**Date**: November 2025 (v4: January 2026)

### 1. Core Information
- **Research Question**: Can causal VAE generate counterfactual financial time series preserving causal relationships?
- **Key Contributions**:
  - TNCM-VAE: Time-series Neural Causal Model VAE
  - Combines VAE with structural causal models
  - DAG constraints in decoder
  - Causal Wasserstein distance for training

### 2. Architecture Details
- **Model Type**: VAE + Structural Causal Model
- **Input Features**: Financial time series
- **Output**: Counterfactual time series
- **Architecture**:
  - VAE encoder-decoder
  - DAG constraints in decoder
  - Causal Wasserstein distance
- **Training**: Causal-constrained VAE training

### 3. Data & Experiments
- **Dataset**: Synthetic AR models (Ornstein-Uhlenbeck inspired)
- **Time Period**: Not applicable (synthetic)
- **Baselines**: Non-causal generative models
- **Metrics**: Counterfactual probability estimation (L1 distance)
- **Results**: L1 distances 0.03-0.10 vs. ground truth

### 4. Code
- **GitHub**: Not specified
- **Framework**: Not specified
- **Libraries**: Not specified

### 5. Key Insights
- **Innovation**: Causal VAE for market simulation
- **Best Use Case**: Stress testing, scenario analysis, backtesting
- **Computational Requirements**: Moderate-High
- **Production Readiness**: Low-Medium (early stage)

---

# SUMMARY TABLES

## Architecture Comparison

| Architecture Type | Papers | Best Use Case | Production Readiness |
|------------------|--------|---------------|---------------------|
| Transformer | 8 | Price prediction, LOB forecasting | Medium-High |
| LSTM/RNN | 7 | Time series, volatility | High |
| GNN | 4 | Multi-asset relationships | Medium |
| Hybrid (CNN+Transformer, etc.) | 5 | Complex pattern recognition | Medium |
| Diffusion | 1 | LOB volume generation | Medium |
| Mamba | 1 | Real-time, long sequences | High |
| Causal Models | 3 | Scenario analysis, risk | Low-Medium |
| Ensemble | 3 | Robust prediction | Medium |

## Code Availability

| Paper | arXiv ID | GitHub Repo | Framework |
|-------|----------|-------------|-----------|
| SAMBA | 2410.03707 | github.com/Ali-Meh619/SAMBA | Likely PyTorch |
| TLOB | 2502.15757 | github.com/LeonardoBerti00/TLOB | Likely PyTorch |
| LOBFrame | 2403.09267 | Open-source codebase | Not specified |

## Performance Highlights

| Metric | Best Result | Paper | Architecture |
|--------|-------------|-------|--------------|
| Sharpe Ratio (gross) | 4+ | 2510.11616 | Attention Factors |
| Sharpe Ratio (net) | 2.3 | 2510.11616 | Attention Factors |
| MSE Reduction | 10.6% | 2502.15813 | LSTM-GNN |
| F1 Improvement | +366% | 2510.20221 | KG+LLM NOTEARS |
| Accuracy | 94% | 2411.05790 | LSTM |
| Return (backtest) | 15.38% | 2507.09347 | Causal Inference |

---

# KEY TAKEAWAYS

## 1. Architecture Trends
- **Transformers dominate** but face competition from Mamba (efficiency)
- **Hybrid architectures** consistently outperform single-model approaches
- **GNNs emerging** for multi-asset and relationship modeling
- **Causal methods** represent next frontier (still research-grade)

## 2. Data & Features
- **LOB data** remains critical for HFT applications
- **Multi-asset features** improving generalization
- **Future covariates** underutilized but promising (CITRAS)
- **Signature features** showing value for path-dependent problems

## 3. Production Considerations
- **Code availability limited**: Only 2-3 papers provide public repos
- **Computational requirements vary widely**: Mamba offers efficiency alternative to Transformers
- **Interpretability improving**: GARCH-RNN hybrids, TINs provide transparency
- **Simulation quality critical**: LOB simulation papers enable better RL training

## 4. Research Gaps
- **Transaction cost modeling**: Often simplified or absent
- **Market impact**: Rarely incorporated in training
- **Cross-asset generalization**: Underexplored except in VWAP papers
- **Causal production systems**: Still experimental

## 5. Recommended Starting Points
For practitioners:
1. **TLOB** (2502.15757) - Best Transformer for LOB, code available
2. **SAMBA** (2410.03707) - Efficient Mamba alternative, code available
3. **Attention Factors** (2510.11616) - Exceptional Sharpe ratios
4. **GARCH-GRU** (2504.09380) - Interpretable volatility forecasting
5. **LOBFrame** (2403.09267) - Open-source LOB forecasting toolkit

---

**Analysis Complete**: 29 papers analyzed across Deep Learning, GNN, Transformers, and Advanced Architectures for trading applications.
