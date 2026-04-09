# ADR-001: Pragmatic Clean Architecture

* **Status:** Accepted
* **Date:** 2026-04-09

## Context

The system is a small, self-contained service requiring the implementation of a Citation Guardrail Engine within a constrained timeframe (~2 hours). It features:
- One core endpoint (`POST /guardrail`)
- Deterministic business rules (R1–R5)
- No database or persistence layer
- Limited domain complexity

We needed to choose an architectural pattern that balances maintainability, testability, and speed of delivery. We considered three paths:

1. **Flat Scripting:** Putting all logic inside FastAPI routers. *(Rejected: Unmaintainable, violates SOLID).*
2. **Strict Enterprise Clean Architecture:** Using extensive Factory patterns, Dependency Injection containers, and strict one-class-per-file rules across deep folder trees. *(Rejected: Over-engineering for the given scope).*
3. **Pragmatic Clean Architecture:** Maintaining strict boundary separations (API, Domain, Infrastructure, Matching) but minimizing unnecessary abstractions and grouping highly cohesive data structures.

## Decision

We will adopt the **Pragmatic Clean Architecture** (Path 3). We will strictly separate concerns into four main layers, while avoiding excessive abstractions (e.g., complex Factory patterns or heavy Dependency Injection containers):

- **API Layer (`Voiceflip/api/`):** Handles HTTP delivery, routing, and strict Pydantic schemas (`schemas.py`).
- **Domain Layer (`Voiceflip/domain/`):** Contains pure business logic, R1-R5 rules orchestration (`guardrail_engine.py`), and highly cohesive dataclasses (`models.py`).
- **Matching Layer (`Voiceflip/matching/`):** Encapsulates the algorithmic link selection strategies (Keyword, Semantic, Hybrid) using a lightweight Strategy pattern.
- **Infrastructure Layer (`Voiceflip/infra/`):** Manages external integrations like HuggingFace and OpenAI embeddings using simple environment-based instantiation.

## Consequences

### Positive
- **Speed & Clarity:** Easier to understand, maintain, and faster to implement under time constraints.
- **Clear Boundaries:** Maintains strict separation of concerns where it matters (e.g., isolating business logic from HTTP routing).
- **High Cohesion:** Grouping cohesive data structures reduces file fragmentation and cognitive load.

### Negative
- **Limited Extensibility:** Less extensible than a full Enterprise architecture. If the application scope drastically expands (e.g., multiple domains, database persistence), refactoring may be required to introduce more rigorous DI patterns.
- **Minor Duplication:** Some structural duplication may exist without the use of generic, multi-layered factories.

## Rationale

The primary goal is to optimize for **clarity, correctness, and evaluation** rather than long-term, large-scale system scalability. The pragmatic approach perfectly balances the required rigor of R1-R5 deterministic rules with the simplicity needed for a focused, single-endpoint RAG post-processing service.