# Durable Workflow Runtime

TruthLayer now has a persisted workflow execution foundation in the private production application.

## Shipped capability

The runtime records every job as durable product state with:

- owner-scoped execution
- explicit job type and subject
- queued, running, review-required, completed, failed, and blocked states
- named execution stages and progress
- attempt and retry limits
- human approval gates
- structured inputs and outputs
- failure reasons
- queue, start, completion, and update timestamps
- an audit event for every state transition

The initial production workflow covers evidence extraction, continuity-report generation, and connector synchronization. Connector jobs remain visibly blocked until provider OAuth and webhook configuration are authorized.

## Governing rules

1. A workflow is not complete because a model returned text.
2. Every transition must be persisted before the next step is exposed.
3. Human review gates cannot be bypassed by the worker that produced the result.
4. Blocked external dependencies must remain explicit rather than being simulated.
5. Retries must be bounded and observable.
6. All state changes must be attributable in the audit history.

## Next runtime layer

The next production increment adds distributed queue consumption, scheduled execution, idempotency keys, leases and heartbeats, dead-letter recovery, connector webhooks, structured model routing, and production observability.

The proprietary application source remains outside this public repository until its visibility is changed to private.
