# Abuse Taxonomy

Use this inventory after mapping the feature. Mark each surface applicable, protected, vulnerable, or not evidenced.

| Surface | Questions to answer | Evidence to inspect |
|---|---|---|
| Identity | Which accountable actor exists: account, credential, invitation, token, device, or only a network signal? Can anonymous, invited, disabled, or compromised actors invoke it? | Guards, token scope, session validation, account state |
| Authorization | Can one role or tenant access another tenant's resources? | Tenant-scoped queries, object keys, signed URLs |
| Volume | What are the sustained and burst request ceilings? Are multi-scope admissions consumed all-or-nothing? | Shared composite limiters, gateway limits, route middleware |
| Concurrency | Can parallel requests bypass a sequential check or exhaust workers? | Atomic consume, locks, queue concurrency |
| Payload | Are actual bytes, item counts, duration, decompression, and parse depth bounded? | Schemas, storage policies, bounded streams |
| Replay | Can the same intent create duplicate storage, jobs, charges, or notifications? Which transport costs still occur on an exact replay? | Transport versus business quotas, idempotency keys, unique constraints, job IDs |
| Retries | How many endpoint, queue, worker, SDK, fallback, and provider attempts occur? When do automatic transient retries pause without deleting durable data? | Retry policies, retry horizon, backoff, paused state, dead-letter handling |
| Cost amplification | How many paid operations can one accepted request trigger? | AI stages, fallbacks, tools, webhooks, media transforms |
| Queue pressure | Is admission bounded independently from worker throughput? | Queue depth, concurrency, age, backpressure |
| Storage | Can abandoned or unconfirmed objects accumulate indefinitely? | Lifecycle rules, cleanup jobs, retention states |
| Egress | Can signed downloads or exports generate unbounded transfer cost? | URL lifetime, download authorization, quotas |
| Multi-tenancy | Can one actor exhaust a tenant-wide or global allowance or affect another tenant? | Actor, tenant, and global quotas; noisy-neighbor isolation |
| Offline replay | Does reconnection create a burst or turn throttling into permanent failure? | Durable queue, retry deadline, attempt accounting |
| Dependency outage | Does limiter, queue, storage, or provider failure become fail-open? | Error mapping, circuit breakers, degraded mode |
| AI input | Can untrusted content override instructions, invoke tools, or exfiltrate data? | Prompt boundaries, tool policy, output schema |
| AI output | Are tokens, schema complexity, and downstream actions bounded? | Token caps, structured output, validation |
| Retention | Is there a lifecycle for completed, failed, and abandoned data? | Retention policy, deletion workflow, legal requirements |
| Observability | Can abuse be detected and attributed without exposing sensitive data? | Metrics, audit events, alerts, redaction |

## Exposure equation

Calculate the upper bound explicitly:

```text
accepted intents
× automatic attempts per intent
× paid stages per attempt
× provider fallbacks or tool calls
= maximum paid operations
```

Repeat for bytes stored, bytes read, queue work, notifications, and egress. Use every applicable accountable-actor, tenant, and global window. A limit that exists only at the first endpoint does not automatically bound downstream retries.

## Severity guide

| Severity | Meaning |
|---|---|
| Critical | Cross-tenant exposure, uncontrolled spend, destructive action, or practical service-wide exhaustion |
| High | Material cost or availability impact reachable by one valid account |
| Medium | Bounded impact, tenant-local denial of service, or missing operational guardrail |
| Low | Defense-in-depth or observability gap with limited direct impact |

Do not assign likelihood from intuition. State required access, effort, and existing prerequisites.
