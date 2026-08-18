---
name: prevent-feature-abuse
description: Use when auditing or implementing a feature exposed to excessive requests, uploads, retries, concurrency, queues, external API or AI spend, tenant quota exhaustion, replay, or offline resubmission, especially before rollout or after adding rate limiting.
---

# Prevent Feature Abuse

## Overview

Bound the harm an actor can cause across the complete execution chain. Rate limiting is one control; the outcome is a measurable ceiling on cost, resource use, and data loss.

## Scope and authority

- Audit requests authorize inspection only. Fix requests authorize in-scope changes through test-driven development.
- Read repository instructions and existing controls first.
- Separate pre-existing findings from feature regressions.

## Workflow

1. **Map the flow.** Trace client, API, storage, queue, worker, provider, retention, and cleanup. Mark trust, actor and tenant boundaries, idempotency, retries, and offline replay.
2. **Compute exposure.** Bound requests, concurrency, bytes, retention, attempts, provider calls, tokens, and spend. An unproven bound is a finding.
3. **Inventory threats.** Read [abuse-taxonomy.md](references/abuse-taxonomy.md) and test every applicable surface.
4. **Select controls.** Read [control-patterns.md](references/control-patterns.md). Limit the real resource, not client-declared metadata.
5. **Verify.** Read [verification-matrix.md](references/verification-matrix.md) before changes. Prove boundaries, outages, retries, replay, and offline behavior red then green.

## Non-negotiable invariants

- For scaled services, evaluate and consume every applicable actor, tenant, and global admission ceiling in one shared all-or-nothing operation. A denied or unavailable decision changes no counter and authorizes no downstream work. Treat IP as a secondary signal when shared networks are legitimate.
- Meter every request for the transport resources it actually consumes, including requests, bytes, concurrency, authentication, and response work. After current authorization and proof of an exact idempotent replay, skip only duplicate business-admission quotas and downstream work that will not run again.
- Verify actual bytes, duration, tokens, and work.
- Include every retry and downstream stage in exposure.
- Fail closed when an unavailable limiter would otherwise permit unmetered external cost or irreversible work.
- Preserve durable data on `429` or temporary `503`; honor server guidance and use a bounded automatic transient-retry budget separate from permanent-failure attempts. When that budget is exhausted, pause for explicit resumable recovery instead of deleting data or retrying forever.
- For critical ingestion, accept data only within an independently enforced durable-ingress budget, then gate costly processing separately.
- Cap AI time, tokens, concurrency, retries, and provider spend.
- Never invent exact limits or present unsupported numbers as "safe defaults." Derive them from evidenced capacity, provider constraints, legitimate bursts, product policy, and acceptable exposure; otherwise name the required variables and data, and mark the limit provisional or the rollout blocked.

## Red flags

Stop rollout or report a blocker when any applies:

- Worst-case cost, storage, or work cannot be stated.
- A signed upload trusts only the declared size.
- An endpoint quota ignores queue or provider retries.
- A costly path fails open when its limiter is unavailable.
- A replay bypasses transport, byte, concurrency, authentication, or response-cost controls.
- Actor, tenant, and global admission checks can partially consume counters or depend on best-effort refunds.
- `429` becomes permanent data loss, consumes permanent-failure attempts, or causes unbounded automatic retries.
- Payload reads, model output, queue depth, or resource retention are unbounded.

## Rationalization checks

| Rationalization | Required response |
|---|---|
| "We added rate limiting" | Recalculate the full downstream amplification chain. |
| "Idempotent means free" | Meter every request's real transport cost; skip only duplicate business effects after replay is proven. |
| "Each scope is atomic" | Make the complete multi-scope admission decision atomic and all-or-nothing. |
| "Availability requires fail-open" | Separate durable ingestion from costly processing. |
| "We must retry forever to preserve data" | Preserve the data, bound automatic retries, then pause for explicit resumable recovery. |
| "We need a safe default now" | Do not invent a number. State the exposure variable, authoritative constraint, and production evidence needed to choose it. |
| "The client declared the size" | Enforce the real size at storage and bounded-read boundaries. |

## Output contract

Lead with the verdict, then provide:

1. Exposure and assumptions.
2. Existing controls with `file:line` evidence.
3. Findings: severity, attack path, impact, control, and test.
4. Changes, evidenced configured limits or unresolved limit variables, and user-visible behavior.
5. Residual risks, operational controls, and fresh verification results.

Never invent traffic, cost, capacity, retry, retention, or quota figures. Calling a number configurable, conservative, or recommended does not make it evidenced. Label sourced estimates and request the missing production data needed to tune limits.
