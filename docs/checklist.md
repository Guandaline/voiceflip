# File Implementation List:

## [DOMAIN-AND-SCHEMAS]
1. voiceclip/domain/models.py : Domain dataclasses (CandidateLink, CitationDecision, Metrics).
2. voiceclip/api/schemas.py : Strict Pydantic models for Request/Response.

## [INFRASTRUCTURE]
3. voiceclip/infrastructure/embeddings.py : Lean provider for HuggingFace and OpenAI based on environment variables.

## [MATCHING-STRATEGIES]
4. voiceclip/matching/base.py : Abstract interface BaseMatchingStrategy.
5. voiceclip/matching/keyword.py : Lexical strategy implementation (fast, deterministic).
6. voiceclip/matching/semantic.py : Semantic implementation with built-in fallback for provider failures.
7. voiceclip/matching/hybrid.py : Combination of 0.7 semantic + 0.3 lexical.

## [CORE-ENGINE]
8. voiceclip/domain/guardrail_engine.py : The core of the system. Orchestrates R1-R5 rules, handles ambiguity, and calls the matching strategy.

## [API-VOICECLIP]
9. voiceclip/api/guardrail_router.py : POST /guardrail route.
10. voiceclip/api/health_router.py : GET /health route (counters).
11. voiceclip/main.py : FastAPI setup, simple dependency injection, and route registration.