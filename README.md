# VoiceFlip: Citation Guardrail Engine

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com)
[![MkDocs Material](https://img.shields.io/badge/docs-MkDocs-indigo)](https://squidfunk.github.io/mkdocs-material/)

A deterministic, observable RAG post-processing microservice built for the VoiceFlip Senior AI Engineer technical test. It enforces strict citation rules (R1-R5) using swappable matching strategies and guarantees graceful degradation to prevent LLM hallucinations.

👉 **[Watch the Video Walkthrough Here](#)** *(Link to be added)*

---

## 📋 Deliverables Overview

* **Video Walkthrough:** Linked above.
* **Extended Golden Set:** Located at `tests/golden_set.json` (10+ cases covering ambiguity, threshold margins, and fallback simulations).
* **Evaluation Script:** Located at `scripts/eval.py`.
* **Architecture Decisions:** Fully documented using ADRs. See the `docs/adrs/` directory or run the documentation server.
* **Design Notes & Trade-offs:** Available in `NOTES.md`.

---

## 🚀 Quick Start

### 1. Installation

Requires Python 3.11+.

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt