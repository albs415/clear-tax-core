# Signed ingestion boundary

TruthLayer accepts provider deliveries only through a provider-neutral signed webhook contract. The production application keeps the implementation and all secrets in its private source and runtime.

## Required envelope

Each delivery includes a connector identifier, an event identifier, a millisecond timestamp, and an HMAC SHA-256 signature over `timestamp.raw_body`.

The runtime rejects missing or invalid signatures, timestamps outside a five-minute replay window, duplicate provider event identifiers, and regulated payloads without an approved processing basis.

## Processing invariants

1. Preserve the original payload in private object storage.
2. Hash the original and write an idempotent durable receipt.
3. Deduplicate by provider event, content hash, and normalized thread identity.
4. Screen instruction injection and payment-redirection attempts before classification.
5. Redact tax and financial identifiers before any model boundary.
6. Record model provider, pinned version, hashes, schema validity, redaction counts, and failure state without storing the raw prompt in the model audit ledger.
7. Derive reliance predicates and permitted actions only in deterministic code.
8. Persist cursor, heartbeat, failures, and retry state so a revoked or stale connector cannot fail silently.

## Current release boundary

The signed runtime is active in synthetic-only mode. Gmail, Microsoft Graph, document-system OAuth, and regulated tax-document processing require provider authorization and an established Section 7216 processing basis before activation. This distinction is enforced in code and must not be represented as a completed connection.