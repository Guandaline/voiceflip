# Citation Guardrail Engine

**A highly deterministic, resilient, and observable post-processing microservice for Retrieval-Augmented Generation (RAG) systems.**

This engine acts as a strict validation layer between the Synthesis LLM and the end-user. Instead of relying on probabilistic language models to inject knowledge base citations—which introduces latency, cost, and hallucination risks—this service enforces deterministic business rules (R1-R5) via code. 

Built with a **Pragmatic Clean Architecture**, the system decouples HTTP routing, domain logic, and infrastructure. It features a pluggable Strategy Pattern for link matching (Keyword, Semantic, and Hybrid) and guarantees high availability through **Graceful Degradation**, automatically falling back to lexical matching if the underlying embedding provider (HuggingFace or OpenAI) experiences an outage.

### Key Architectural Highlights
* **Zero Hallucination Guarantee:** Citations are only injected if strict grounding and threshold criteria are met.
* **Graceful Degradation:** Embedding failures (timeout/500) trigger an automatic fallback to deterministic Keyword Matching without dropping the user request.
* **Pluggable Strategies:** Easily swappable `Keyword`, `Semantic`, and `Hybrid` matching algorithms.
* **Observability-First:** Full telemetry on latency, LLM calls, and decision-making reasoning attached to every payload.