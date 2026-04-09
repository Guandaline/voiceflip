# VoiceFlip: Citation Guardrail Engine

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Poetry](https://img.shields.io/badge/Poetry-Enabled-blueviolet)](https://python-poetry.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com)
[![MkDocs Material](https://img.shields.io/badge/docs-MkDocs-indigo)](https://squidfunk.github.io/mkdocs-material/)

A deterministic, observable RAG post-processing microservice built for the VoiceFlip Senior AI Engineer technical test. It enforces strict citation rules (R1-R5) using swappable matching strategies and guarantees graceful degradation to prevent LLM hallucinations.

👉 **[Watch the Video Walkthrough Here](https://drive.google.com/file/d/18MiYt-p3SRTLNcfNpOQfve6e1ZIBCJh5/view?usp=sharing)**

---

## 📋 Deliverables Overview

* **Video Walkthrough:** Linked above.
* **Extended Golden Set:** Located at `benchmarks/golden_set.json` (10+ cases covering ambiguity, threshold margins, and fallback simulations).
* **Evaluation Script:** Located at `scripts/eval.py`.
* **Architecture Decisions:** Fully documented using ADRs. See the `docs/adrs/` directory or run the documentation server.
* **Design Notes & Trade-offs:** Available in `NOTES.md` (which includes references to our detailed Architecture Decision Records in `docs/adrs/`).

---

## 🚀 Quick Start

### Prerequisites
* **Python 3.11+**
* **[Poetry](https://python-poetry.org/docs/#installation)** for dependency management.

### 1. Installation

We use a **Makefile** to simplify common tasks. To install all dependencies and set up the virtual environment, run:

```bash
make install
```
*(Alternatively, you can run `poetry install` directly).*

### 2. Environment Setup

Copy the sample environment file and adjust it if necessary (the default offline HuggingFace settings work out of the box):

```bash
cp .env.sample .env
```

### 3. Running the API

To start the FastAPI server locally on port 8000:

```bash
make run
```

### 4. Running the Evaluation

To execute the local evaluation script against the 10+ edge cases in the golden set:

```bash
poetry run python scripts/eval.py
```

### 5. Test output

```text

ID                               | EXPECTED           | ACTUAL             | RESULT
------------------------------------------------------------------------------------------
seed_01_grounded_inject          | injected           | injected           | PASS
seed_02_grounded_already_present | already_present    | already_present    | PASS
seed_03_chitchat_skip            | skipped_chitchat   | skipped_chitchat   | PASS
seed_04_ungrounded_skip          | skipped_ungrounded | skipped_ungrounded | PASS
seed_05_no_match                 | skipped_no_match   | skipped_no_match   | PASS
edge_06_empty_candidates         | skipped_no_match   | skipped_no_match   | PASS
edge_07_low_threshold            | skipped_no_match   | skipped_no_match   | PASS
edge_08_ambiguous_keywords       | injected           | injected           | PASS
edge_09_similar_scores_margin    | skipped_no_match   | injected           | FAIL (Rule R4: Injected via keyword)
edge_10_duplication_plain_text   | already_present    | already_present    | PASS
------------------------------------------------------------------------------------------
Final Accuracy: 90.00% (9/10)
------------------------------------------------------------------------------------------
```