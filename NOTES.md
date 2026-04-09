# Design Notes & Trade-offs

As requested in the deliverables, here is a high-level summary of the core decisions. 

> **Note to Reviewers:** For a deep dive into the systemic resilience, fallback mechanisms, and architectural structure of this engine, please review the formal Architecture Decision Records located in the `docs/adrs/` directory.

### 1. Chosen Strategy & Why
**Hybrid Matching Strategy (Semantic + Keyword)**
I chose a Hybrid approach to balance deep understanding with deterministic reliability. The Semantic strategy (using embeddings and cosine similarity) excels at capturing intent even when vocabulary differs (e.g., mapping "login issues" to a "Password Reset" link). However, relying solely on network-bound or compute-heavy embeddings introduces failure points. By combining it with a Keyword strategy—and specifically using the Keyword strategy as an automatic fallback if the embedding provider times out—we guarantee the endpoint never drops a 500 Internal Server Error, preserving the user experience via Graceful Degradation.

### 2. Trade-off Accepted
**Bi-Encoder Speed vs. Cross-Encoder Precision**
For the semantic matching layer, I accepted the trade-off of using a Bi-encoder (`all-MiniLM-L6-v2`) over a Cross-Encoder. While Cross-Encoders provide superior relational understanding for edge cases, they are too slow for a synchronous post-processing guardrail that sits between the LLM synthesis and the user. The Bi-encoder allows us to maintain sub-100ms latency. To mitigate the loss in precision, I introduced an `ambiguity_margin` parameter: if the top two candidates have very similar scores, the system safely aborts the injection to prevent false positives.

### 3. Limitation
**Naive Lexical Tokenization**
The current `KeywordMatchingStrategy` uses a highly simplified regex-based tokenizer (`re.findall(r"\w+", text.lower())`). While deterministic and zero-dependency, this approach is strictly limited to English or languages with clear whitespace boundaries. It does not perform stemming/lemmatization, meaning "listing" and "listings" are treated as distinct tokens. Furthermore, it would completely fail on languages like Japanese or Chinese. In a production environment, this would need to be replaced with a robust analyzer (like an inverted index query via Elasticsearch/OpenSearch or an NLP library like spaCy).