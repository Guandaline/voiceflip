# File Implementation List:

## [DOMAIN-AND-SCHEMAS]
1. Voiceflip/domain/models.py : Domain dataclasses (CandidateLink, CitationDecision, Metrics).
2. Voiceflip/api/schemas.py : Strict Pydantic models for Request/Response.

## [INFRASTRUCTURE]
3. Voiceflip/infrastructure/embeddings.py : Lean provider for HuggingFace and OpenAI based on environment variables.

## [MATCHING-STRATEGIES]
4. Voiceflip/matching/base.py : Abstract interface BaseMatchingStrategy.
5. Voiceflip/matching/keyword.py : Lexical strategy implementation (fast, deterministic).
6. Voiceflip/matching/semantic.py : Semantic implementation with built-in fallback for provider failures.
7. Voiceflip/matching/hybrid.py : Combination of 0.7 semantic + 0.3 lexical.

## [CORE-ENGINE]
8. Voiceflip/domain/guardrail_engine.py : The core of the system. Orchestrates R1-R5 rules, handles ambiguity, and calls the matching strategy.

## [API-VOICEFLIP]
9. Voiceflip/api/guardrail_router.py : POST /guardrail route.
10. Voiceflip/api/health_router.py : GET /health route (counters).
11. Voiceflip/main.py : FastAPI setup, simple dependency injection, and route registration.