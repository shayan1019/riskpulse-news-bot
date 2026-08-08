# RiskPulse — Real-Time Financial News & Geopolitical Risk Telegram Bot

> **Public Portfolio Showcase & Architecture Overview**
> 
> *RiskPulse is a trader-focused risk monitoring bot designed to ingest real-time macro, geopolitical, commodity, and cross-asset news, classify event severity, calculate symbol impact biases, and broadcast localized Telegram alerts before market shock damage compounds.*

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               Multi-Source News Ingestion                               │
│  NewsAPI.org  ·  GDELT DOC API  ·  Marketaux Market News  ·  Alpha Vantage Sentiment    │
└───────────────────────────┬─────────────────────────────────────────────────────────────┘
                            │ Raw Headlines / Events
                            ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                 RiskPulse Core Engine                                   │
│                                                                                         │
│  1. Normalization      ➜ Standardized to NormalizedEvent schema                         │
│  2. Classification     ➜ Risk Category (Geopolitical, Macro, Gold, Oil, Crypto, CB)      │
│  3. Impact Mapping     ➜ Affected Symbols & Directional Bias Calculation                │
│  4. Priority Scoring   ➜ Score Assignment (LOW, MEDIUM, HIGH, CRITICAL)                 │
│  5. Deduplication      ➜ Fingerprint matching, story clustering, burst cooldowns        │
│  6. Multi-User Engine  ➜ User preference filtering (Category, Severity, Mode)         │
│  7. Localization       ➜ 4 Languages (English, Turkish, Farsi, Arabic)                  │
└───────────────────────────┬─────────────────────────────────────────────────────────────┘
                            │ Filtered & Formatted Messages
                            ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                Telegram Delivery Layer                                  │
│  Multi-User Telegram Subscriber Broadcast · Button-First UI · Interactive Menus         │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Technical Features

- **Multi-Source News Aggregation**: Consolidates news feeds from NewsAPI.org, GDELT, Marketaux, Alpha Vantage, and RSS sources with automatic source gap detection and exponential retry backoff.
- **Rule-Based Risk Classifier**: Categorizes events into Geopolitical Escalation/De-escalation, Central Bank Policy, Inflation/Labor Macro Shocks, Commodity Supply Disruptions, and Crypto Stress.
- **Smart Deduplication & Burst Control**: Fingerprint hashing, semantic story clustering, and configurable cooldown windows prevent notification noise during breaking news spikes.
- **Multi-User Telegram Bot Engine**:
  - Full subscriber registration & preference management via Telegram commands (`/start`, `/subscribe`, `/language`, `/news`, `/level`, `/mode`).
  - Interactive Button-First UX with inline keyboard selectors and state markers.
  - Multi-language localization in English (`en`), Turkish (`tr`), Farsi (`fa`), and Arabic (`ar`).
- **RESTful API Service**: Exposes FastAPI endpoints (`/health`, `/metrics-summary`, `/webhook/external-alert`) for health monitoring and third-party alert ingestion.
- **Deterministic Replay Test Suite**: Includes scenario replay datasets testing critical market events (FOMC surprise, NFP shock, Strait of Hormuz escalation/de-escalation, OPEC production cuts).

---

## Project Structure

```
forexnewsbot/
├── config/
│   └── riskpulse.yaml            # Watchlist, category thresholds, cooldown settings
├── src/riskpulse/
│   ├── adapters/                 # NewsAPI, GDELT, Marketaux, Alpha Vantage adapters
│   ├── app/                      # CLI, polling worker, FastAPI service entrypoints
│   ├── delivery/                 # Telegram Bot API client & localized message templates
│   ├── models/                   # Pydantic schemas (NormalizedEvent, SubscriberConfig)
│   ├── persistence/              # SQLite database storage & subscriber store
│   └── services/                 # Classifier, mapper, scorer, dedup engine
├── scripts/
│   └── replay_scenarios.py       # Deterministic scenario replay validation runner
├── run_riskpulse.py              # Primary single-action startup script
├── pyproject.toml                # Standard Python package manifest
└── tests/                        # Comprehensive unit and scenario tests
```

---

## Quick Start (Demo Mode)

### Prerequisites
- Python 3.11+

### Installation

```powershell
# Clone repo
git clone https://github.com/shayan1019/riskpulse-news-bot.git
cd riskpulse-news-bot

# Set up environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -e .
pip install -r requirements.txt
```

### Environment Configuration
Copy `.env.example` to `.env` and set your credentials:

```ini
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
ENABLE_NEWSAPI=true
NEWSAPI_API_KEY=your_newsapi_key_here
ENABLE_GDELT=true
```

### Run Replay Scenario Test Suite

```powershell
python scripts/replay_scenarios.py
```

### Run Application Tests

```powershell
pytest
```
