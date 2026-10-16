# Derived-market-analysis-tool

Derived-market analysis tool for Deriv options (even/odd, matches/differs, over/under) with bulk accumulators.

This repository contains a Python package and a CLI that supports both paper (simulation) mode and live trading mode via the Deriv API WebSocket.

WARNING: Live trading can lose real money. By default the CLI runs in paper mode. To place real trades you must provide a valid Deriv API token via the DERIV_API_TOKEN environment variable and pass the --mode live flag to the CLI. The CLI will ask for an extra confirmation before placing live trades.

Quick start (paper/demo):

1. Create a virtual environment and install requirements:

   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt

2. Run the demo accumulator in paper mode:

   python -m derived_market_analysis.cli run --config examples/accumulator_example.yaml --mode paper

Quick start (live/demo account):

1. Set the DERIV_API_TOKEN environment variable to your Deriv API token.
2. Run with live mode:

   export DERIV_API_TOKEN="YOUR_TOKEN_HERE"
   python -m derived_market_analysis.cli run --config examples/accumulator_example.yaml --mode live

Repository structure:
- src/derived_market_analysis/accumulator.py — Accumulator dataclass and logic
- src/derived_market_analysis/engine.py — Core engine for paper/live runs
- src/derived_market_analysis/deriv_client.py — Lightweight Deriv WebSocket client (live operations)
- src/derived_market_analysis/simulator.py — Paper-mode tick generation utilities
- src/derived_market_analysis/cli.py — Command-line entrypoint
- examples/accumulator_example.yaml — Example accumulator configuration

See the project docs below for more usage details and safety notes.

Usage examples

1) Paper mode (safe example):

   python -m derived_market_analysis.cli run --config examples/accumulator_example.yaml --mode paper

2) Live mode (will require confirmation):

   export DERIV_API_TOKEN="your-token"
   python -m derived_market_analysis.cli run --config examples/accumulator_example.yaml --mode live

3) CLI help:

   python -m derived_market_analysis.cli --help

Important notes
- The live trading client is intentionally small and intentionally conservative.
- For production work, validate each Deriv contract proposal against the API contract definitions and your broker rules.
- This tool is designed around the following option families:
  - digit even / odd
  - digit matches / differs
  - digit over / under
- The accumulator logic requires that each run uses the same entry/exit spot, same contract type, same stake, and same number of runs, matching your requirement.

This repo is a starting framework for a Deriv option accumulator system. It can be extended with backtesting, indicator logic, additional contract families, and stronger risk controls.
