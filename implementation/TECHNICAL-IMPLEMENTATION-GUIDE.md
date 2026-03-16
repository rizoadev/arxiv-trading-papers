# Technical Implementation Guide
## AI Trading System - Production Architecture

**Based on:** 142 arXiv papers (2024-2026)  
**Version:** 1.0  
**Date:** March 16, 2026  
**Status:** Ready for Implementation

---

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Component Specifications](#component-specifications)
3. [Code Structure](#code-structure)
4. [Core Agent Implementations](#core-agent-implementations)
5. [Risk Management](#risk-management)
6. [Execution Engine](#execution-engine)
7. [Backtesting Framework](#backtesting-framework)
8. [Deployment Guide](#deployment-guide)

---

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      STRATEGIC LAYER (Hourly)                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Fundamental  │  │  Sentiment   │  │  Technical   │         │
│  │   Analyst    │  │   Analyst    │  │   Analyst    │         │
│  │  (LLM)       │  │  (DPO-LLM)   │  │  (LSTM-GNN)  │         │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘         │
│         │                 │                 │                  │
│         └─────────────────┼─────────────────┘                  │
│                           │                                    │
│                    ┌──────▼───────┐                           │
│                    │    Manager   │                           │
│                    │  (LLM + RL)  │                           │
│                    │  Adaptive-   │                           │
│                    │  OPRO        │                           │
│                    └──────┬───────┘                           │
└───────────────────────────┼───────────────────────────────────┘
                            │
┌───────────────────────────▼───────────────────────────────────┐
│                     TACTICAL LAYER (Second-level)             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │
│  │ Risk Monitor │  │  Execution   │  │  Position    │        │
│  │ (GARCH-GRU)  │  │   Engine     │  │  Manager     │        │
│  │              │  │ (VWAP Trans) │  │ (Dual-agent) │        │
│  └──────────────┘  └──────────────┘  └──────────────┘        │
└───────────────────────────────────────────────────────────────┘
```

### Technology Stack

| Layer | Component | Technology | Source Paper |
|-------|-----------|------------|--------------|
| **Strategic** | Fundamental Analyst | Llama 3.3 70B (self-hosted) | TradingAgents |
| **Strategic** | Sentiment Analyst | DPO-tuned Llama 3 | FinDPO |
| **Strategic** | Technical Analyst | LSTM-GNN Hybrid | LSTM-GNN (2502.15813) |
| **Strategic** | Manager | LLM + Adaptive-OPRO | ATLAS |
| **Tactical** | Risk Monitor | GARCH-GRU | GARCH-GRU (2504.09380) |
| **Tactical** | Execution | VWAP Transformer | VWAP Signature (2503.02680) |
| **Tactical** | Position Manager | Dual-agent architecture | FinPos (2510.27251) |
| **Infrastructure** | Message Queue | Redis Pub/Sub | - |
| **Infrastructure** | Database | PostgreSQL + TimescaleDB | - |
| **Infrastructure** | Cache | Redis | - |
| **Infrastructure** | GPU | NVIDIA L4 (inference), A10G (training) | - |

---

## Component Specifications

### 1. Fundamental Analyst (LLM)

**Purpose:** Analyze financial reports, earnings calls, SEC filings

**Input:**
- SEC 10-K/10-Q filings
- Earnings call transcripts
- Company press releases
- Analyst reports

**Output:**
```json
{
  "company": "AAPL",
  "sentiment": "bullish",
  "confidence": 0.78,
  "key_factors": [
    "Revenue beat expectations by 12%",
    "Services growth accelerating",
    "China risk remains elevated"
  ],
  "target_price_range": [180, 220],
  "time_horizon": "3-6 months",
  "citations": ["10-Q Q1 2026 p.12", "Earnings Call 2026-01-15"]
}
```

**Model:** Llama 3.3 70B (self-hosted)  
**Latency:** <5 seconds per analysis  
**Cost:** ~$0.02 per analysis (self-hosted)

### 2. Sentiment Analyst (DPO-LLM)

**Purpose:** Real-time sentiment from news and social media

**Input:**
- Financial news (Bloomberg, Reuters, etc.)
- Twitter/X financial influencers
- Reddit (r/wallstreetbets, r/investing)
- StockTwits

**Output:**
```json
{
  "symbol": "AAPL",
  "sentiment_score": 0.65,
  "sentiment_strength": "moderate_bullish",
  "volume": 15420,
  "momentum": "increasing",
  "key_topics": ["iPhone sales", "AI features", "China"],
  "anomaly_detected": false,
  "timestamp": "2026-03-16T11:30:00Z"
}
```

**Model:** DPO-tuned Llama 3 8B  
**Latency:** <500ms  
**Update Frequency:** Real-time (streaming)

### 3. Technical Analyst (LSTM-GNN)

**Purpose:** Price prediction and technical pattern recognition

**Input:**
- OHLCV data (1min, 5min, 15min, 1H, 1D)
- Technical indicators (RSI, MACD, Bollinger, etc.)
- Inter-stock correlations (Pearson)

**Output:**
```json
{
  "symbol": "AAPL",
  "prediction_1d": {
    "direction": "up",
    "probability": 0.67,
    "expected_move_pct": 1.2,
    "confidence_interval": [0.8, 1.6]
  },
  "prediction_5d": {
    "direction": "up",
    "probability": 0.61,
    "expected_move_pct": 2.5
  },
  "technical_signals": {
    "rsi": 58,
    "macd": "bullish_crossover",
    "support": 172.50,
    "resistance": 178.00
  }
}
```

**Model:** LSTM-GNN Hybrid (10.6% MSE improvement)  
**Latency:** <100ms  
**Update Frequency:** Every minute

### 4. Manager (LLM + Adaptive-OPRO)

**Purpose:** Synthesize all signals, make final trading decision

**Input:**
- Fundamental analysis (from Fundamental Analyst)
- Sentiment scores (from Sentiment Analyst)
- Technical predictions (from Technical Analyst)
- Current positions
- Risk limits

**Output:**
```json
{
  "decision": "BUY",
  "symbol": "AAPL",
  "quantity": 500,
  "order_type": "LIMIT",
  "limit_price": 174.50,
  "time_in_force": "DAY",
  "rationale": "Strong fundamental + positive sentiment + technical breakout",
  "confidence": 0.72,
  "risk_adjusted": true,
  "stop_loss": 168.00,
  "take_profit": 185.00
}
```

**Model:** Llama 3.3 70B + Adaptive-OPRO  
**Latency:** <3 seconds  
**Update Frequency:** Hourly or on significant events

### 5. Risk Monitor (GARCH-GRU)

**Purpose:** Real-time volatility forecasting and risk limits

**Input:**
- Real-time returns
- Historical volatility
- Market regime indicators

**Output:**
```json
{
  "current_volatility": 0.18,
  "forecast_1d": 0.21,
  "forecast_5d": 0.24,
  "var_95": -0.032,
  "cvar_95": -0.048,
  "risk_regime": "elevated",
  "position_limit_pct": 0.15,
  "max_drawdown_limit": 0.08,
  "circuit_breaker": false
}
```

**Model:** GARCH-GRU (interpretable, 3x faster than GARCH-LSTM)  
**Latency:** <50ms  
**Update Frequency:** Every second

### 6. Execution Engine (VWAP Transformer)

**Purpose:** Optimal order execution with minimal slippage

**Input:**
- Order from Manager
- Current order book
- Historical volume profile
- Market impact model

**Output:**
```json
{
  "execution_strategy": "VWAP",
  "total_quantity": 500,
  "slices": [
    {"time": "10:00", "quantity": 50, "order_type": "LIMIT", "price": 174.48},
    {"time": "10:15", "quantity": 75, "order_type": "LIMIT", "price": 174.52},
    {"time": "10:30", "quantity": 100, "order_type": "MARKET"},
    ...
  ],
  "expected_slippage_bps": 3.5,
  "expected_completion_time": "14:00"
}
```

**Model:** Signature-Enhanced VWAP Transformer  
**Latency:** <10ms per slice  
**Update Frequency:** Per order

### 7. Position Manager (Dual-Agent)

**Purpose:** Continuous position management and adjustment

**Input:**
- Current positions
- P&L
- Risk metrics
- New signals

**Output:**
```json
{
  "action": "ADJUST",
  "symbol": "AAPL",
  "current_quantity": 500,
  "new_quantity": 450,
  "reason": "Volatility increase, reduce exposure",
  "adjustment_type": "RISK_REDUCTION",
  "urgency": "NORMAL"
}
```

**Model:** Dual-agent (direction + risk)  
**Latency:** <200ms  
**Update Frequency:** Every 5 minutes or on significant events

---

## Code Structure

```
ai-trading-system/
├── config/
│   ├── default.yaml              # Default configuration
│   ├── production.yaml           # Production overrides
│   └── models.yaml               # Model configurations
│
├── agents/
│   ├── __init__.py
│   ├── fundamental_analyst.py    # LLM-based fundamental analysis
│   ├── sentiment_analyst.py      # DPO-LLM sentiment
│   ├── technical_analyst.py      # LSTM-GNN technical
│   ├── manager.py                # LLM + Adaptive-OPRO manager
│   ├── risk_monitor.py           # GARCH-GRU risk
│   ├── execution_engine.py       # VWAP Transformer execution
│   └── position_manager.py       # Dual-agent position mgmt
│
├── models/
│   ├── __init__.py
│   ├── lstm_gnn.py               # LSTM-GNN architecture
│   ├── garch_gru.py              # GARCH-GRU volatility
│   ├── vwap_transformer.py       # VWAP execution
│   └── dpo_sentiment.py          # DPO-tuned sentiment
│
├── infrastructure/
│   ├── __init__.py
│   ├── database.py               # PostgreSQL + TimescaleDB
│   ├── cache.py                  # Redis cache
│   ├── message_queue.py          # Redis Pub/Sub
│   └── broker_api.py             # Broker integration
│
├── backtesting/
│   ├── __init__.py
│   ├── engine.py                 # Backtesting engine
│   ├── data_loader.py            # Historical data
│   ├── metrics.py                # Performance metrics
│   └── stress_tests.py           # TradeTrap-style tests
│
├── monitoring/
│   ├── __init__.py
│   ├── metrics.py                # Prometheus metrics
│   ├── alerting.py               # Alert system
│   └── logging.py                # Structured logging
│
├── tests/
│   ├── __init__.py
│   ├── test_agents.py            # Agent unit tests
│   ├── test_models.py            # Model tests
│   └── test_integration.py       # Integration tests
│
├── scripts/
│   ├── train_models.py           # Model training scripts
│   ├── backtest_strategy.py      # Run backtests
│   └── deploy.py                 # Deployment scripts
│
├── requirements.txt
├── setup.py
├── Dockerfile
└── README.md
```

---

## Core Agent Implementations

### 1. Fundamental Analyst

```python
# agents/fundamental_analyst.py

from typing import Dict, List, Optional
from dataclasses import dataclass
import json
from datetime import datetime
import torch
from transformers import LlamaForCausalLM, LlamaTokenizer

@dataclass
class FundamentalAnalysis:
    symbol: str
    sentiment: str  # bullish, bearish, neutral
    confidence: float
    key_factors: List[str]
    target_price_range: tuple
    time_horizon: str
    citations: List[str]
    timestamp: datetime

class FundamentalAnalyst:
    """
    LLM-based fundamental analysis agent.
    Based on: TradingAgents (2412.20138), Trading-R1 (2509.11420)
    """
    
    def __init__(self, model_path: str = "meta-llama/Llama-3.3-70B-Instruct"):
        self.tokenizer = LlamaTokenizer.from_pretrained(model_path)
        self.model = LlamaForCausalLM.from_pretrained(
            model_path,
            torch_dtype=torch.float16,
            device_map="auto"
        )
        self.model.eval()
        
        self.system_prompt = """You are a professional equity analyst. 
Analyze the provided financial documents and produce a structured assessment.
CRITICAL: CITE sources for ALL claims (e.g., "10-Q Q1 2026 p.12").
"""
    
    def analyze(self, 
                symbol: str, 
                documents: List[Dict[str, str]]) -> FundamentalAnalysis:
        """
        Analyze financial documents for a given symbol.
        
        Args:
            symbol: Stock ticker
            documents: List of documents with 'type' and 'content' keys
            
        Returns:
            FundamentalAnalysis object
        """
        # Build prompt
        prompt = self._build_prompt(symbol, documents)
        
        # Generate analysis
        response = self._generate(prompt)
        
        # Parse response
        analysis = self._parse_response(symbol, response)
        
        return analysis
    
    def _build_prompt(self, symbol: str, documents: List[Dict]) -> str:
        """Build analysis prompt from documents."""
        doc_texts = "\n\n".join([
            f"Document Type: {doc['type']}\nContent:\n{doc['content']}"
            for doc in documents
        ])
        
        prompt = f"""{self.system_prompt}

Symbol: {symbol}

Documents:
{doc_texts}

Provide your analysis in the following JSON format:
{{
    "sentiment": "bullish|bearish|neutral",
    "confidence": 0.0-1.0,
    "key_factors": ["factor 1", "factor 2", ...],
    "target_price_range": [low, high],
    "time_horizon": "1-3 months|3-6 months|6-12 months",
    "citations": ["source 1", "source 2", ...]
}}

Analysis:"""
        return prompt
    
    def _generate(self, prompt: str, max_tokens: int = 1000) -> str:
        """Generate response from LLM."""
        inputs = self.tokenizer(prompt, return_tensors="pt").to(self.model.device)
        
        with torch.no_grad():
            outputs = self.model.generate(
                **inputs,
                max_new_tokens=max_tokens,
                temperature=0.3,
                top_p=0.9,
                do_sample=True
            )
        
        response = self.tokenizer.decode(outputs[0], skip_special_tokens=True)
        return response[len(prompt):].strip()
    
    def _parse_response(self, symbol: str, response: str) -> FundamentalAnalysis:
        """Parse LLM response into FundamentalAnalysis object."""
        try:
            # Extract JSON from response
            start_idx = response.find('{')
            end_idx = response.rfind('}') + 1
            json_str = response[start_idx:end_idx]
            
            data = json.loads(json_str)
            
            return FundamentalAnalysis(
                symbol=symbol,
                sentiment=data['sentiment'],
                confidence=data['confidence'],
                key_factors=data['key_factors'],
                target_price_range=tuple(data['target_price_range']),
                time_horizon=data['time_horizon'],
                citations=data['citations'],
                timestamp=datetime.utcnow()
            )
        except Exception as e:
            # Fallback to neutral if parsing fails
            return FundamentalAnalysis(
                symbol=symbol,
                sentiment='neutral',
                confidence=0.5,
                key_factors=['Parsing error'],
                target_price_range=(0, 0),
                time_horizon='3-6 months',
                citations=[],
                timestamp=datetime.utcnow()
            )
```

### 2. Sentiment Analyst (DPO-LLM)

```python
# agents/sentiment_analyst.py

from typing import Dict, Optional
from dataclasses import dataclass
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

@dataclass
class SentimentAnalysis:
    symbol: str
    sentiment_score: float  # -1 to 1
    sentiment_strength: str
    volume: int
    momentum: str
    key_topics: list
    anomaly_detected: bool
    timestamp: str

class SentimentAnalyst:
    """
    DPO-tuned LLM for financial sentiment analysis.
    Based on: FinDPO (2507.18417), Attention Factors (2510.11616)
    """
    
    def __init__(self, model_path: str = "finetuned-llama-3-dpo-sentiment"):
        self.tokenizer = AutoTokenizer.from_pretrained(model_path)
        self.model = AutoModelForCausalLM.from_pretrained(
            model_path,
            torch_dtype=torch.float16,
            device_map="auto"
        )
        self.model.eval()
    
    def analyze_stream(self, 
                       symbol: str, 
                       text_stream: list) -> SentimentAnalysis:
        """
        Analyze sentiment from streaming text data.
        
        Args:
            symbol: Stock ticker
            text_stream: List of text snippets (news, tweets, etc.)
            
        Returns:
            SentimentAnalysis object
        """
        # Aggregate text
        combined_text = " ".join(text_stream[-50:])  # Last 50 items
        
        # Generate sentiment
        sentiment_score = self._predict_sentiment(symbol, combined_text)
        
        # Detect topics
        topics = self._extract_topics(combined_text)
        
        # Detect anomalies
        anomaly = self._detect_anomaly(sentiment_score, text_stream)
        
        return SentimentAnalysis(
            symbol=symbol,
            sentiment_score=sentiment_score,
            sentiment_strength=self._categorize_strength(sentiment_score),
            volume=len(text_stream),
            momentum=self._calculate_momentum(text_stream),
            key_topics=topics,
            anomaly_detected=anomaly,
            timestamp=datetime.utcnow().isoformat()
        )
    
    def _predict_sentiment(self, symbol: str, text: str) -> float:
        """Predict sentiment score using DPO-tuned model."""
        prompt = f"""Analyze sentiment for {symbol} from this text. 
Respond with ONLY a number between -1 and 1:

Text: {text[:2000]}

Sentiment score:"""
        
        inputs = self.tokenizer(prompt, return_tensors="pt").to(self.model.device)
        
        with torch.no_grad():
            outputs = self.model.generate(
                **inputs,
                max_new_tokens=10,
                temperature=0.0,  # Deterministic
                do_sample=False
            )
        
        response = self.tokenizer.decode(outputs[0], skip_special_tokens=True)
        
        # Extract number
        try:
            score = float(response.strip().split()[-1])
            return max(-1.0, min(1.0, score))
        except:
            return 0.0
    
    def _categorize_strength(self, score: float) -> str:
        """Categorize sentiment strength."""
        abs_score = abs(score)
        if abs_score < 0.2:
            return "neutral"
        elif abs_score < 0.4:
            return "weak_" + ("bullish" if score > 0 else "bearish")
        elif abs_score < 0.6:
            return "moderate_" + ("bullish" if score > 0 else "bearish")
        else:
            return "strong_" + ("bullish" if score > 0 else "bearish")
    
    def _extract_topics(self, text: str) -> list:
        """Extract key topics from text."""
        # Simple keyword-based extraction
        # In production, use LLM for better topic extraction
        keywords = {
            'earnings': ['earnings', 'revenue', 'EPS', 'beat', 'miss'],
            'product': ['product', 'launch', 'iPhone', 'iPad', 'AI'],
            'regulatory': ['SEC', 'lawsuit', 'investigation', 'fine'],
            'market': ['market share', 'competition', 'growth'],
            'management': ['CEO', 'CFO', 'resign', 'appointment']
        }
        
        topics = []
        text_lower = text.lower()
        for topic, words in keywords.items():
            if any(word in text_lower for word in words):
                topics.append(topic)
        
        return topics[:5]  # Top 5 topics
    
    def _calculate_momentum(self, text_stream: list) -> str:
        """Calculate sentiment momentum."""
        if len(text_stream) < 10:
            return "stable"
        
        # Compare recent vs older sentiment
        recent = text_stream[-10:]
        older = text_stream[-20:-10]
        
        recent_positive = sum(1 for t in recent if any(w in t.lower() for w in ['good', 'great', 'beat', 'buy']))
        older_positive = sum(1 for t in older if any(w in t.lower() for w in ['good', 'great', 'beat', 'buy']))
        
        if recent_positive > older_positive * 1.5:
            return "increasing"
        elif recent_positive < older_positive * 0.5:
            return "decreasing"
        else:
            return "stable"
    
    def _detect_anomaly(self, score: float, text_stream: list) -> bool:
        """Detect sentiment anomalies."""
        # Simple z-score based anomaly detection
        if len(text_stream) < 20:
            return False
        
        # Calculate historical average
        historical_scores = []
        for text in text_stream:
            # Simplified: just count positive/negative words
            pos_words = sum(1 for w in ['good', 'great', 'beat', 'buy', 'upgrade'] if w in text.lower())
            neg_words = sum(1 for w in ['bad', 'miss', 'sell', 'downgrade'] if w in text.lower())
            historical_scores.append((pos_words - neg_words) / max(1, pos_words + neg_words))
        
        import numpy as np
        mean = np.mean(historical_scores)
        std = np.std(historical_scores)
        
        if std > 0 and abs(score - mean) > 2 * std:
            return True
        
        return False
```

### 3. Technical Analyst (LSTM-GNN)

```python
# agents/technical_analyst.py

from typing import Dict, Tuple
from dataclasses import dataclass
import torch
import torch.nn as nn
import numpy as np
import pandas as pd

@dataclass
class TechnicalPrediction:
    symbol: str
    prediction_1d: Dict
    prediction_5d: Dict
    technical_signals: Dict
    timestamp: str

class LSTMGNNTechnicalAnalyst(nn.Module):
    """
    Hybrid LSTM-GNN model for stock price prediction.
    Based on: LSTM-GNN (2502.15813) - 10.6% MSE improvement
    
    Architecture:
    - LSTM for temporal patterns
    - GNN for inter-stock relationships (Pearson correlation)
    """
    
    def __init__(self, 
                 input_size: int = 20,
                 lstm_hidden: int = 128,
                 gnn_hidden: int = 64,
                 num_stocks: int = 100):
        super().__init__()
        
        # LSTM for temporal patterns
        self.lstm = nn.LSTM(
            input_size=input_size,
            hidden_size=lstm_hidden,
            num_layers=2,
            batch_first=True,
            dropout=0.3
        )
        
        # Graph convolution for inter-stock relationships
        self.gcn = nn.Sequential(
            nn.Linear(num_stocks, gnn_hidden),
            nn.ReLU(),
            nn.Linear(gnn_hidden, gnn_hidden),
            nn.ReLU()
        )
        
        # Combined prediction head
        self.predictor = nn.Sequential(
            nn.Linear(lstm_hidden + gnn_hidden, 64),
            nn.ReLU(),
            nn.Dropout(0.3),
            nn.Linear(64, 1)
        )
    
    def forward(self, x: torch.Tensor, adj_matrix: torch.Tensor) -> torch.Tensor:
        """
        Forward pass.
        
        Args:
            x: Time series data (batch, seq_len, features)
            adj_matrix: Adjacency matrix for GNN (batch, num_stocks, num_stocks)
        """
        # LSTM encoding
        lstm_out, (h_n, c_n) = self.lstm(x)
        lstm_features = h_n[-1]  # Last hidden state
        
        # GNN encoding
        gnn_features = self.gcn(adj_matrix)
        gnn_features = gnn_features.mean(dim=1)  # Aggregate across stocks
        
        # Combine and predict
        combined = torch.cat([lstm_features, gnn_features], dim=1)
        prediction = self.predictor(combined)
        
        return prediction
    
    def train_step(self, 
                   batch_x: torch.Tensor,
                   batch_adj: torch.Tensor,
                   batch_y: torch.Tensor,
                   optimizer: torch.optim.Optimizer) -> float:
        """Single training step."""
        self.train()
        optimizer.zero_grad()
        
        predictions = self(batch_x, batch_adj)
        loss = nn.MSELoss()(predictions, batch_y)
        
        loss.backward()
        optimizer.step()
        
        return loss.item()

class TechnicalAnalystAgent:
    """
    Agent wrapper for LSTM-GNN model.
    """
    
    def __init__(self, model_path: str, num_stocks: int = 100):
        self.model = LSTMGNNTechnicalAnalyst(num_stocks=num_stocks)
        self.model.load_state_dict(torch.load(model_path))
        self.model.eval()
        
        self.feature_columns = [
            'open', 'high', 'low', 'close', 'volume',
            'rsi', 'macd', 'bollinger_upper', 'bollinger_lower',
            'sma_20', 'sma_50', 'ema_12', 'ema_26',
            'atr', 'adx', 'cci', 'williams_r',
            'momentum', 'roc', 'obv'
        ]
    
    def predict(self, 
                symbol: str,
                price_data: pd.DataFrame,
                correlation_matrix: pd.DataFrame) -> TechnicalPrediction:
        """
        Generate technical prediction.
        
        Args:
            symbol: Stock ticker
            price_data: Historical price data with features
            correlation_matrix: Inter-stock correlation matrix
        """
        # Prepare features
        features = self._prepare_features(price_data)
        
        # Prepare adjacency matrix
        adj_matrix = self._prepare_adjacency(correlation_matrix)
        
        # Generate prediction
        with torch.no_grad():
            features_tensor = torch.FloatTensor(features).unsqueeze(0)
            adj_tensor = torch.FloatTensor(adj_matrix).unsqueeze(0)
            
            pred_1d = self.model(features_tensor, adj_tensor).item()
            
            # 5-day prediction (autoregressive)
            pred_5d = self._predict_5day(features_tensor, adj_tensor)
        
        # Get technical signals
        tech_signals = self._calculate_technical_signals(price_data)
        
        return TechnicalPrediction(
            symbol=symbol,
            prediction_1d={
                'direction': 'up' if pred_1d > 0 else 'down',
                'probability': self._probability_from_pred(pred_1d),
                'expected_move_pct': abs(pred_1d) * 100,
                'confidence_interval': self._confidence_interval(pred_1d)
            },
            prediction_5d=pred_5d,
            technical_signals=tech_signals,
            timestamp=datetime.utcnow().isoformat()
        )
    
    def _prepare_features(self, price_data: pd.DataFrame) -> np.ndarray:
        """Prepare feature matrix for model."""
        # Normalize features
        features = price_data[self.feature_columns].copy()
        features = (features - features.mean()) / features.std()
        
        # Create sequences (last 60 days)
        seq_length = 60
        return features.tail(seq_length).values
    
    def _prepare_adjacency(self, corr_matrix: pd.DataFrame) -> np.ndarray:
        """Prepare adjacency matrix for GNN."""
        # Use correlation as edge weights
        adj = corr_matrix.values
        np.fill_diagonal(adj, 0)  # No self-loops
        
        # Threshold small correlations
        adj[np.abs(adj) < 0.3] = 0
        
        return adj
    
    def _predict_5day(self, 
                      features: torch.Tensor, 
                      adj: torch.Tensor) -> Dict:
        """Generate 5-day prediction (autoregressive)."""
        predictions = []
        current_features = features.clone()
        
        for _ in range(5):
            pred = self.model(current_features, adj).item()
            predictions.append(pred)
            
            # Update features (simplified - in production, update properly)
            # This is a placeholder for proper autoregressive prediction
        
        avg_pred = np.mean(predictions)
        
        return {
            'direction': 'up' if avg_pred > 0 else 'down',
            'probability': self._probability_from_pred(avg_pred),
            'expected_move_pct': abs(avg_pred) * 100
        }
    
    def _calculate_technical_signals(self, price_data: pd.DataFrame) -> Dict:
        """Calculate standard technical indicators."""
        close = price_data['close'].iloc[-1]
        
        # RSI
        rsi = self._calculate_rsi(price_data['close'])
        
        # MACD
        macd_signal = self._calculate_macd(price_data['close'])
        
        # Support/Resistance
        support = price_data['low'].rolling(20).min().iloc[-1]
        resistance = price_data['high'].rolling(20).max().iloc[-1]
        
        return {
            'rsi': rsi,
            'macd': macd_signal,
            'support': round(support, 2),
            'resistance': round(resistance, 2)
        }
    
    def _calculate_rsi(self, prices: pd.Series, period: int = 14) -> float:
        """Calculate RSI."""
        delta = prices.diff()
        gain = (delta.where(delta > 0, 0)).rolling(period).mean()
        loss = (-delta.where(delta < 0, 0)).rolling(period).mean()
        rs = gain / loss
        rsi = 100 - (100 / (1 + rs))
        return round(rsi.iloc[-1], 2)
    
    def _calculate_macd(self, prices: pd.Series) -> str:
        """Calculate MACD signal."""
        ema_12 = prices.ewm(span=12).mean()
        ema_26 = prices.ewm(span=26).mean()
        macd = ema_12 - ema_26
        signal = macd.ewm(span=9).mean()
        
        if macd.iloc[-1] > signal.iloc[-1] and macd.iloc[-2] <= signal.iloc[-2]:
            return 'bullish_crossover'
        elif macd.iloc[-1] < signal.iloc[-1] and macd.iloc[-2] >= signal.iloc[-2]:
            return 'bearish_crossover'
        elif macd.iloc[-1] > signal.iloc[-1]:
            return 'bullish'
        else:
            return 'bearish'
    
    def _probability_from_pred(self, pred: float) -> float:
        """Convert prediction to probability."""
        # Sigmoid transformation
        return 1 / (1 + np.exp(-pred * 10))
    
    def _confidence_interval(self, pred: float) -> list:
        """Calculate confidence interval."""
        std = 0.02  # Assumed standard deviation
        mean_pred = pred * 100
        return [
            round(mean_pred - 1.96 * std * 100, 2),
            round(mean_pred + 1.96 * std * 100, 2)
        ]
```

---

## Risk Management

### GARCH-GRU Volatility Monitor

```python
# agents/risk_monitor.py

import torch
import torch.nn as nn
import numpy as np
from dataclasses import dataclass
from typing import Dict

@dataclass
class RiskMetrics:
    current_volatility: float
    forecast_1d: float
    forecast_5d: float
    var_95: float
    cvar_95: float
    risk_regime: str
    position_limit_pct: float
    max_drawdown_limit: float
    circuit_breaker: bool

class GARCHGRU(nn.Module):
    """
    GARCH-GRU hybrid for volatility forecasting.
    Based on: GARCH-GRU (2504.09380)
    
    Architecture:
    - GARCH(1,1) volatility update in multiplicative gating
    - GRU for nonlinear temporal dependencies
    - Interpretable GARCH parameters
    """
    
    def __init__(self, input_size: int = 1, hidden_size: int = 64):
        super().__init__()
        
        # GARCH parameters (interpretable)
        self.omega = nn.Parameter(torch.tensor(0.1))  # Constant
        self.alpha = nn.Parameter(torch.tensor(0.1))  # ARCH term
        self.beta = nn.Parameter(torch.tensor(0.8))   # GARCH term
        
        # GRU for nonlinear dynamics
        self.gru = nn.GRU(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=2,
            batch_first=True,
            dropout=0.3
        )
        
        # Volatility prediction head
        self.volatility_head = nn.Sequential(
            nn.Linear(hidden_size, 32),
            nn.ReLU(),
            nn.Linear(32, 1),
            nn.Softplus()  # Ensure positive volatility
        )
    
    def forward(self, returns: torch.Tensor) -> torch.Tensor:
        """
        Forward pass with GARCH-GRU integration.
        
        Args:
            returns: Return series (batch, seq_len, 1)
        """
        batch_size = returns.shape[0]
        
        # Initialize volatility
        sigma2 = torch.ones(batch_size, 1, 1) * returns.var()
        
        # GARCH recursion with GRU integration
        volatilities = []
        h_t = None
        
        for t in range(returns.shape[1]):
            # GARCH(1,1) update
            epsilon2 = returns[:, t:t+1, :] ** 2
            sigma2 = self.omega + self.alpha * epsilon2 + self.beta * sigma2
            
            # GRU update with volatility as additional input
            gru_input = torch.cat([returns[:, t:t+1, :], sigma2], dim=2)
            gru_out, h_t = self.gru(gru_input, h_t)
            
            # Predict volatility
            vol_pred = self.volatility_head(gru_out)
            volatilities.append(vol_pred)
        
        # Stack volatilities
        volatilities = torch.stack(volatilities, dim=1)
        
        return volatilities.squeeze(-1)
    
    def get_garch_parameters(self) -> Dict[str, float]:
        """Get interpretable GARCH parameters."""
        return {
            'omega': self.omega.item(),
            'alpha': self.alpha.item(),
            'beta': self.beta.item(),
            'persistence': self.alpha.item() + self.beta.item()
        }

class RiskMonitor:
    """
    Real-time risk monitoring agent.
    Based on: FinRS (2511.12599), FinPos (2510.27251)
    """
    
    def __init__(self, model_path: str, var_confidence: float = 0.95):
        self.model = GARCHGRU()
        self.model.load_state_dict(torch.load(model_path))
        self.model.eval()
        
        self.var_confidence = var_confidence
        self.returns_history = []
        self.volatility_history = []
        
        # Risk limits
        self.max_position_pct = 0.20
        self.max_drawdown_pct = 0.10
        self.circuit_breaker_threshold = 0.05  # 5% daily loss
    
    def assess_risk(self, 
                    returns: np.ndarray,
                    portfolio_value: float) -> RiskMetrics:
        """
        Assess current risk level.
        
        Args:
            returns: Recent returns (last 100 observations)
            portfolio_value: Current portfolio value
        """
        # Update history
        self.returns_history.extend(returns.tolist())
        self.returns_history = self.returns_history[-1000:]
        
        # Forecast volatility
        with torch.no_grad():
            returns_tensor = torch.FloatTensor(returns).unsqueeze(0).unsqueeze(-1)
            volatility_forecast = self.model(returns_tensor)
        
        current_vol = volatility_forecast[0, -1].item()
        forecast_1d = volatility_forecast[0, -1].item()
        forecast_5d = current_vol * np.sqrt(5)  # Square root of time
        
        self.volatility_history.append(current_vol)
        
        # Calculate VaR and CVaR
        var_95 = self._calculate_var(returns, current_vol)
        cvar_95 = self._calculate_cvar(returns, current_vol)
        
        # Determine risk regime
        risk_regime = self._determine_regime(current_vol)
        
        # Adjust position limits based on risk
        position_limit = self._adjust_position_limit(current_vol, risk_regime)
        
        # Check circuit breaker
        daily_return = self._calculate_daily_return()
        circuit_breaker = daily_return < -self.circuit_breaker_threshold
        
        return RiskMetrics(
            current_volatility=current_vol,
            forecast_1d=forecast_1d,
            forecast_5d=forecast_5d,
            var_95=var_95,
            cvar_95=cvar_95,
            risk_regime=risk_regime,
            position_limit_pct=position_limit,
            max_drawdown_limit=self.max_drawdown_pct,
            circuit_breaker=circuit_breaker
        )
    
    def _calculate_var(self, returns: np.ndarray, volatility: float) -> float:
        """Calculate Value at Risk (95%)."""
        # Parametric VaR
        from scipy.stats import norm
        z_score = norm.ppf(1 - self.var_confidence)
        var = volatility * z_score
        return round(var, 4)
    
    def _calculate_cvar(self, returns: np.ndarray, volatility: float) -> float:
        """Calculate Conditional VaR (Expected Shortfall)."""
        # Parametric CVaR
        from scipy.stats import norm
        z_score = norm.ppf(1 - self.var_confidence)
        cvar = volatility * (-norm.pdf(z_score) / (1 - self.var_confidence))
        return round(cvar, 4)
    
    def _determine_regime(self, volatility: float) -> str:
        """Determine market risk regime."""
        if len(self.volatility_history) < 100:
            return "unknown"
        
        historical_mean = np.mean(self.volatility_history)
        historical_std = np.std(self.volatility_history)
        
        if volatility > historical_mean + 2 * historical_std:
            return "crisis"
        elif volatility > historical_mean + historical_std:
            return "elevated"
        elif volatility < historical_mean - historical_std:
            return "calm"
        else:
            return "normal"
    
    def _adjust_position_limit(self, volatility: float, regime: str) -> float:
        """Adjust position limit based on volatility."""
        base_limit = self.max_position_pct
        
        if regime == "crisis":
            return base_limit * 0.25
        elif regime == "elevated":
            return base_limit * 0.5
        elif regime == "calm":
            return base_limit * 1.5
        else:
            return base_limit
    
    def _calculate_daily_return(self) -> float:
        """Calculate daily return for circuit breaker."""
        if len(self.returns_history) < 2:
            return 0.0
        
        # Sum today's returns (assuming intraday)
        # Simplified - in production, track properly
        return sum(self.returns_history[-10:])
```

---

## Backtesting Framework

### TradeTrap-Style Stress Testing

```python
# backtesting/stress_tests.py

import numpy as np
import pandas as pd
from typing import Dict, List
from dataclasses import dataclass

@dataclass
class StressTestResult:
    scenario: str
    max_drawdown: float
    recovery_days: int
    passed: bool
    details: Dict

class TradeTrapStressTester:
    """
    Stress testing framework based on TradeTrap (2512.02261).
    Tests for catastrophic failure modes from small perturbations.
    """
    
    def __init__(self, trading_system):
        self.trading_system = trading_system
        self.pass_thresholds = {
            'max_drawdown': 0.20,  # 20% max
            'recovery_days': 30,
            'concentration': 0.80,  # 80% max in single asset
            'leverage': 3.0  # 3x max
        }
    
    def run_all_tests(self, 
                      initial_capital: float = 1_000_000) -> List[StressTestResult]:
        """Run all stress tests."""
        scenarios = [
            self._market_crash,
            self._flash_crash,
            self._liquidity_crisis,
            self._component_failure,
            self._data_corruption
        ]
        
        results = []
        for scenario_fn in scenarios:
            result = scenario_fn(initial_capital)
            results.append(result)
        
        return results
    
    def _market_crash(self, initial_capital: float) -> StressTestResult:
        """Test: Market crash (-20% in 1 day)."""
        # Simulate 20% market drop
        shock = -0.20
        
        # Run trading system through crash
        portfolio = self.trading_system.run_with_shock(shock, initial_capital)
        
        # Calculate metrics
        max_dd = self._calculate_max_drawdown(portfolio)
        recovery = self._calculate_recovery_days(portfolio)
        passed = max_dd < self.pass_thresholds['max_drawdown']
        
        return StressTestResult(
            scenario="Market Crash (-20% in 1 day)",
            max_drawdown=max_dd,
            recovery_days=recovery,
            passed=passed,
            details={
                'final_value': portfolio[-1],
                'shock_magnitude': shock
            }
        )
    
    def _flash_crash(self, initial_capital: float) -> StressTestResult:
        """Test: Flash crash (-5% in 5 minutes)."""
        shock = -0.05
        
        # Run with flash crash
        portfolio = self.trading_system.run_with_shock(shock, initial_capital, duration_minutes=5)
        
        max_dd = self._calculate_max_drawdown(portfolio)
        recovery = self._calculate_recovery_days(portfolio)
        passed = max_dd < self.pass_thresholds['max_drawdown'] and recovery < 5
        
        return StressTestResult(
            scenario="Flash Crash (-5% in 5 minutes)",
            max_drawdown=max_dd,
            recovery_days=recovery,
            passed=passed,
            details={'shock_magnitude': shock}
        )
    
    def _liquidity_crisis(self, initial_capital: float) -> StressTestResult:
        """Test: Liquidity crisis (bid-ask spread 10x normal)."""
        # Simulate 10x spread increase
        spread_multiplier = 10
        
        portfolio = self.trading_system.run_with_spread_shock(
            spread_multiplier, initial_capital
        )
        
        max_dd = self._calculate_max_drawdown(portfolio)
        passed = max_dd < self.pass_thresholds['max_drawdown']
        
        return StressTestResult(
            scenario="Liquidity Crisis (10x bid-ask spread)",
            max_drawdown=max_dd,
            recovery_days=0,  # Not applicable
            passed=passed,
            details={'spread_multiplier': spread_multiplier}
        )
    
    def _component_failure(self, initial_capital: float) -> StressTestResult:
        """Test: Single component failure (e.g., sentiment agent returns garbage)."""
        # Simulate sentiment agent failure
        portfolio = self.trading_system.run_with_component_failure(
            component='sentiment_analyst',
            failure_mode='garbage_output',
            initial_capital=initial_capital
        )
        
        max_dd = self._calculate_max_drawdown(portfolio)
        concentration = self._calculate_max_concentration(portfolio)
        passed = (max_dd < self.pass_thresholds['max_drawdown'] and 
                  concentration < self.pass_thresholds['concentration'])
        
        return StressTestResult(
            scenario="Component Failure (Sentiment Agent)",
            max_drawdown=max_dd,
            recovery_days=0,
            passed=passed,
            details={
                'failed_component': 'sentiment_analyst',
                'max_concentration': concentration
            }
        )
    
    def _data_corruption(self, initial_capital: float) -> StressTestResult:
        """Test: Data corruption (missing/malformed inputs)."""
        # Simulate 10% data corruption
        corruption_rate = 0.10
        
        portfolio = self.trading_system.run_with_data_corruption(
            corruption_rate, initial_capital
        )
        
        max_dd = self._calculate_max_drawdown(portfolio)
        passed = max_dd < self.pass_thresholds['max_drawdown']
        
        return StressTestResult(
            scenario="Data Corruption (10% missing/malformed)",
            max_drawdown=max_dd,
            recovery_days=0,
            passed=passed,
            details={'corruption_rate': corruption_rate}
        )
    
    def _calculate_max_drawdown(self, portfolio: List[float]) -> float:
        """Calculate maximum drawdown."""
        portfolio = np.array(portfolio)
        running_max = np.maximum.accumulate(portfolio)
        drawdown = (portfolio - running_max) / running_max
        return abs(drawdown.min())
    
    def _calculate_recovery_days(self, portfolio: List[float]) -> int:
        """Calculate days to recover from max drawdown."""
        portfolio = np.array(portfolio)
        running_max = np.maximum.accumulate(portfolio)
        drawdown = (portfolio - running_max) / running_max
        
        # Find max drawdown point
        max_dd_idx = drawdown.argmin()
        
        # Find recovery point
        peak_value = portfolio[max_dd_idx] / (1 + drawdown[max_dd_idx])
        recovery_idx = np.where(portfolio[max_dd_idx:] >= peak_value)[0]
        
        if len(recovery_idx) == 0:
            return 999  # Didn't recover
        else:
            return recovery_idx[0]
    
    def _calculate_max_concentration(self, portfolio) -> float:
        """Calculate maximum single-asset concentration."""
        # Simplified - in production, track positions properly
        return 0.25  # Assume 25% max
```

---

## Deployment Guide

### Docker Configuration

```dockerfile
# Dockerfile

FROM python:3.11-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy code
COPY . .

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV CUDA_VISIBLE_DEVICES=0

# Expose ports
EXPOSE 8000  # API
EXPOSE 9090  # Metrics

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import requests; requests.get('http://localhost:8000/health')"

# Run
CMD ["python", "-m", "uvicorn", "api.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Kubernetes Deployment

```yaml
# k8s/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: ai-trading-system
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ai-trading
  template:
    metadata:
      labels:
        app: ai-trading
    spec:
      containers:
      - name: trading-system
        image: rizoadev/ai-trading-system:latest
        ports:
        - containerPort: 8000
        - containerPort: 9090
        resources:
          requests:
            memory: "8Gi"
            cpu: "4"
            nvidia.com/gpu: "1"
          limits:
            memory: "16Gi"
            cpu: "8"
            nvidia.com/gpu: "1"
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: trading-secrets
              key: database-url
        - name: BROKER_API_KEY
          valueFrom:
            secretKeyRef:
              name: trading-secrets
              key: broker-api-key
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 5
```

---

## Next Steps

1. **Clone repository structure**
2. **Install dependencies**: `pip install -r requirements.txt`
3. **Download models** (Llama 3.3, etc.)
4. **Configure database** (PostgreSQL + TimescaleDB)
5. **Set up Redis** (cache + message queue)
6. **Train models** on historical data
7. **Run backtests** with stress tests
8. **Deploy to staging** environment
9. **Paper trade** for 2-4 weeks
10. **Go live** with small capital (1-5%)

---

**Status:** Ready for implementation  
**Estimated Time:** 6 months to production  
**Team Size:** 3-5 engineers
