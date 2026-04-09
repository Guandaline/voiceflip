# ADR-003: Graceful Degradation on Embedding Failure

* **Status:** Accepted
* **Date:** 2026-04-09

## Context

The guardrail engine depends on external infrastructure (either a HuggingFace local inference model or the OpenAI remote API) to calculate semantic similarity. Network partitions, API rate limits, or local resource exhaustion can cause these embedding calls to fail or timeout. A failure in the guardrail layer should not cause the entire upstream chat application to return a 500 Internal Server Error to the end user.

## Decision

We decided to implement Graceful Degradation in the Semantic and Hybrid matching strategies. 
If the embedding provider raises an exception or times out:
1. The error will be caught and logged.
2. The strategy will immediately fall back to the deterministic pure Keyword Strategy.
3. The response will succeed, but the decision reason will explicitly record the failure (e.g., `reason="embedding_failed_fallback_keyword"`).

## Consequences

* **Positive:**
    * The API guarantees high availability and will never return a hard 500 error due to third-party embedding outages.
    * The system remains deterministic and observable, as the fallback event is clearly recorded in the response payload.
* **Negative:**
    * During an outage, the quality of citation matching will temporarily degrade to pure lexical matching, potentially missing semantic nuances.
* **Neutral/Other:**
    * System monitors must be set up to alert on the `embedding_failed_fallback_keyword` metric to ensure outages are not silently ignored.