---
name: prevent-feature-abuse
description: Audit or harden a feature against abuse, cost amplification, quota exhaustion, replay, unsafe retries, and unbounded resource use.
---

# Prevent Feature Abuse

## Overview

Bound the harm an actor can cause across the complete execution chain. Rate limiting is one control; the outcome is a measurable ceiling on cost, resource use, and data loss.

## Scope and authority

- Audit requests authorize inspection only. Fix requests authorize in-scope controls and verification. For changed protections, prefer regression tests that expose the missing control; configuration-only changes can use the owning validator.
- Read repository instructions and existing controls first.
- Separate pre-existing findings from feature regressions.

## Workflow

1. **Map the flow.** Trace the feature's applicable stages across client, API, storage, queue, worker, provider, retention, and cleanup. Mark trust, actor and tenant boundaries, idempotency, retries, and offline replay.
2. **Compute exposure.** Bound requests, concurrency, bytes, retention, attempts, provider calls, tokens, and spend. Distinguish evidenced unbounded behavior from missing capacity or policy evidence; state the latter as an unresolved exposure estimate.
3. **Inventory threats.** Use [abuse-taxonomy.md](references/abuse-taxonomy.md) for an audit's threat inventory; assess applicable surfaces with evidence.
4. **Select controls.** Read relevant patterns in [control-patterns.md](references/control-patterns.md) when proposing or implementing controls. Limit the real resource, not client-declared metadata.
5. **Verify.** For fixes or a requested test plan, use applicable rows of [verification-matrix.md](references/verification-matrix.md). For read-only audits, report evidence and missing checks without creating tests or implementing controls.

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

## Findings and rollout

Report a concrete attack path and impact for demonstrated missing controls. When capacity or policy evidence is unavailable, identify the missing input and provisional exposure separately; do not present an unknown as a proven exploit. Recommend blocking the affected rollout when an essential bound cannot be established, while continuing independent audit or hardening work. Audit findings do not authorize deployment changes.

## Output contract

Lead with the verdict, then provide:

1. Exposure and assumptions.
2. Existing controls with `file:line` evidence.
3. Findings: severity, attack path, impact, control, and test.
4. Changes, evidenced configured limits or unresolved limit variables, and user-visible behavior.
5. Residual risks, operational controls, and fresh verification results.

Never invent traffic, cost, capacity, retry, retention, or quota figures. Calling a number configurable, conservative, or recommended does not make it evidenced. Label sourced estimates and request the missing production data needed to tune limits.
