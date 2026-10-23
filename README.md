# 🧾 Document Intelligence

> Turn scanned receipts and invoices into **validated, structured JSON** —
> and measure how accurate the extraction really is, field by field.

[![CI](https://github.com/abinthomas9322/document-intelligence/actions/workflows/ci.yml/badge.svg)](https://github.com/abinthomas9322/document-intelligence/actions/workflows/ci.yml)
[![Python 3.12](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](pyproject.toml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**Status: in progress** — built in planned slices, each shipped as its own
tested, reviewed commit. See [Roadmap](#roadmap).

## Why this project

Finance, insurance and pharma teams in Ireland still process mountains of
receipts, invoices and statements by hand. This project is a small but
production-shaped version of the system that replaces that work:

1. a document (image or PDF) goes in,
2. an extractor pulls the fields a downstream system needs
   (merchant, date, total, line items …),
3. every result is **schema-validated** (Pydantic) before anything is stored,
4. and an **evaluation harness** reports exact-match accuracy per field
   against a labelled dataset, so a prompt or model change can never silently
   make things worse.

Two extractor strategies are compared on the same labelled data:

| Strategy | How it works |
|---|---|
| **OCR + text LLM** | On-device OCR (ONNX, no GPU) → text LLM fills the schema |
| **Vision LLM** | The image itself goes to a multimodal model that fills the schema |

The results table below is filled in as each slice ships.

## Results (measured)

_Coming with slice 1 (baseline) and slice 2 (vision)._

| Extractor | Field exact-match | Docs fully correct | Median latency | Cost / 1k docs |
|---|---|---|---|---|
| OCR + text LLM | – | – | – | – |
| Vision LLM | – | – | – | – |

## Roadmap

| # | Slice | What it proves | Status |
|---|---|---|---|
| 1 | Dataset, schema, OCR+LLM baseline, eval harness | Structured output, evaluation discipline | 🔧 in progress |
| 2 | Vision-LLM extractor + comparison table | Multimodal LLMs, retries, schema repair | ⏳ |
| 3 | FastAPI service (`POST /extract`), SQLite store, batch | Production API design | ⏳ |
| 4 | Review UI: upload, see fields, correct mistakes | Human-in-the-loop | ⏳ |
| 5 | Docker, CI eval gate, per-request cost/latency tracing | MLOps / observability | ⏳ |
| 6 | Live deployment, final results, write-up | Shipping | ⏳ |

## Quick start

```bash
python -m venv .venv
.venv/Scripts/python -m pip install -r requirements.txt -r requirements-dev.txt   # Windows
cp .env.example .env    # add your keys
pytest
```

## Project layout

```
docintel/      library code (schemas, extractors, evaluation)
eval/          labelled golden set + `python -m eval.run`
tests/         unit tests, 100% coverage enforced in CI
docs/          design notes and decisions
data/          local datasets (git-ignored)
```

## Engineering practices

- Ruff lint + format, mypy strict, bandit, pip-audit, gitleaks — all in CI
- 100% test coverage gate; LLM calls are faked in tests, measured in `eval/`
- Pinned dependencies, no secrets in the repo, MIT licence

## Progress

- **Slice 0** — repository skeleton, CI, roadmap.
