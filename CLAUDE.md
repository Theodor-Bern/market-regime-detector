# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

End-to-end ML platform for real-time market regime detection and trading signal generation. Uses S&P 500, VIX, and 10-year Treasury yield data to classify market conditions and produce signals.

## Commands

```bash
# Install (editable with dev dependencies)
pip install -e ".[dev]"

# Run data fetcher
python -m src.data.fetcher

# Run tests
pytest

# Run a single test file
pytest tests/unit/test_fetcher.py

# Lint
ruff check .

# Format
ruff format .

# Type check
mypy src/
```

## Architecture

The pipeline flows through these layers, each in its own `src/` subpackage:

1. **`src/data/`** — Fetches OHLCV market data via `yfinance` and economic data via `fredapi`. Saves to `data/raw/*.parquet`.
2. **`src/features/`** — Feature engineering from raw data into `feature_store/` *(stub)*.
3. **`src/models/`** — Regime detection ML models *(stub)*.
4. **`src/serving/`** — API serving layer for predictions *(stub)*.
5. **`src/monitoring/`** — Model performance monitoring *(stub)*.

**Streaming:** Docker Compose includes a Redpanda (Kafka-compatible) broker for real-time data ingestion.

## Key Files

- `src/data/fetcher.py` — Only implemented module; fetches S&P 500 (`^GSPC`), VIX (`^VIX`), and 10Y Treasury (`^TNX`), outputs parquet to `data/raw/`.
- `pyproject.toml` — Dependencies, ruff config (line length 88, py311 target), pytest config.
- `docker-compose.yml` — Defines `app` and `redpanda` services.
- `.env` — Contains `FRED_API_KEY` (required for FRED economic data).

## Environment

- Python 3.11+
- `FRED_API_KEY` env var required for FRED API access (set in `.env`).
- Parquet files use `pyarrow` as the backend.
- Structured logging via `structlog`; settings/config via `pydantic-settings`.
