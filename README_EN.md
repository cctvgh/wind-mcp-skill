# Wind Financial Data (wind-mcp-skill)

Query Wind financial data service through a local CLI, organized by data domain: equities, funds / ETFs, indices / sectors, bonds, announcements and news, macroeconomic and industry indicators, and cross-instrument aggregation.

## What it is

Lets an agent answer financial-data questions with structured, traceable data instead of model memory or generic web sources. Answers rely only on returned values — no filler commentary — and include a data-source attribution line on success.

## Features

- **Seven-domain routing**: `stock_data`, `fund_data`, `index_data`, `bond_data`, `financial_docs`, `economic_data`, `analytics_data`. Each domain has one contract document; parameter names and field values come strictly from it.
- **Structured envelopes**: successes return data objects; failures return an error envelope with a `code` (`AUTH_ERROR`, `PARAM_VALIDATION_ERROR`, `RATE_LIMIT_ERROR`, etc.) for precise correction.
- **Batch and concurrency limits**: price-indicator tools accept up to 50 codes per call; calls are serial by default and capped at 10 concurrent only on explicit request, degrading immediately on rate limits.
- **Diagnosable**: built-in `diagnose`, `list-tools`, `open-portal` and `setup-key` subcommands, with clear guidance when the key is missing.
- **No build step**: plain Node.js ESM scripts with no third-party packages.

## Quick start

1. Requirements: Node.js 18+ (20+ recommended).

2. Configure an API key (any of the following, highest priority first):

```bash
# 1) Interactive setup
node scripts/cli.mjs setup-key YOUR_WIND_KEY --scope global

# 2) Environment variable
export WIND_API_KEY=YOUR_WIND_KEY      # Windows PowerShell: $env:WIND_API_KEY="YOUR_WIND_KEY"

# 3) User-level config file (default: ~/.wind-aifinmarket/config)
```

3. Example calls:

```bash
node scripts/cli.mjs call stock_data get_stock_price_indicators '{"windcode":"600519.SH"}'
node scripts/cli.mjs call index_data get_index_quote '{"windcode":"000300.SH"}'
node scripts/cli.mjs list-tools stock_data
```

4. On non-POSIX shells (PowerShell / cmd), write UTF-8 JSON to `scripts/request-<suffix>.json` and pass `@scripts/request-<suffix>.json`, deleting it afterwards.

## Directory layout

```
wind-mcp-skill/
├── SKILL.md                 # Entry point: route → call → read receipt → wrap up
├── references/              # One contract per data domain plus indicator field lists
├── scripts/
│   ├── cli.mjs              # Main entry (call / list-tools / setup-key / diagnose)
│   ├── update-check.mjs     # Update checker
│   ├── tool-manifest.json   # Tool inventory
│   └── call-rules.json      # Call rules
├── README.md
├── README_EN.md
├── LICENSE
└── .gitignore
```

> Runtime state files (e.g. `update-state.json`) and local key configs are not distributed and are ignored by `.gitignore`.

## License

MIT License. See [LICENSE](LICENSE).

## Disclaimer

This skill provides a data access channel only and does not constitute investment advice. Data copyright belongs to the service provider; comply with its terms and licensing scope. Quotes and financials may be delayed or defined differently — make investment decisions independently and at your own risk. Never commit an API key to a repository.