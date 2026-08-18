# Control Patterns

Apply controls at the boundary that owns the resource being protected.

## Quick reference

| Risk | Primary control | Required companion control |
|---|---|---|
| Repeated requests | Transport quotas on every request | Idempotent business admission and `Retry-After` |
| Parallel bypass | Composite all-or-nothing multi-scope consume | Concurrency and zero-partial-debit tests |
| Oversized upload | Signed exact length or range policy | Bounded server read and hash |
| Duplicate jobs | Stable job ID or unique outbox event | Idempotent worker |
| Retry amplification | Bounded automatic transient-attempt budget | Backoff, paused recovery, permanent-failure budget, provider cap |
| AI spend | Admission quota and concurrency | Timeout, token cap, spend ceiling |
| Tenant exhaustion | Per-actor and per-tenant/global limits | Admin visibility and tuning data |
| Offline burst | Durable local queue | Bounded jittered reschedule and paused recovery |
| Orphaned storage | State-aware cleanup or lifecycle | Retention metric and alert |
| Dependency outage | Explicit fail policy | Alert and tested degraded behavior |

## Choosing fail-open or fail-closed

Use this decision order:

1. If allowing the operation creates unmetered external spend, irreversible effects, or cross-tenant risk, fail closed.
2. If rejecting would lose safety-critical or user-authored data, persist the intent durably only when an independent item, byte, age, and capacity budget authorizes admission; do not execute the costly effect yet.
3. If the operation is read-only, cheap, reversible, and isolated, fail-open may be acceptable only with an explicit exposure bound and alert.

Never treat ingestion and processing as one indivisible availability decision. Durable acceptance can remain available within its own authoritative capacity envelope while AI, exports, notifications, or other costly work stays gated.

## Rate-limiter properties

- Use a store shared by every instance.
- Consume every applicable actor, tenant, and global admission ceiling in one all-or-nothing operation before costly work. A denied or unavailable decision changes no counter and authorizes no downstream work.
- Key by the narrowest accountable actor available; prefer credentials, invitations, tokens, or devices over IP when shared networks are legitimate.
- Meter every request for transport resources it actually consumes: request rate, bytes, concurrency, authentication, lookup, and response or egress work.
- Return a bounded retry duration.
- Authenticate and apply current authorization before returning a stored result. After proving the same actor, tenant, route, idempotency key, and canonical payload, skip only duplicate business-admission quotas and downstream work that will not run again.
- Use an idempotent admission decision when the limiter response can be lost, so a retry observes the original outcome without a second debit.
- Test store errors separately from limit exhaustion.
- Choose windows from legitimate workflow and worst-case exposure, then tune from observed data.

## Retry properties

- Keep transient-attempt accounting separate from permanent business failures.
- Bound automatic retries by attempts, elapsed horizon, or both, as required by the exposure equation.
- Honor bounded server retry guidance; otherwise use capped exponential backoff with jitter.
- When the automatic budget is exhausted, enter a durable paused or needs-attention state that can be resumed explicitly.
- Never delete user-authored data merely because a transient retry budget is exhausted.
- Do not supply default attempts, horizons, delays, or backoff caps without evidence; name the configuration variables and the production inputs needed to set them.

## Payload properties

- Treat filenames, MIME types, checksums, duration, counts, and sizes from clients as untrusted claims.
- Constrain uploads at storage, then verify using a bounded stream.
- Stop reading at `limit + 1`; never hash, parse, or decompress an unbounded object first.
- Bound nested arrays, archive expansion, image dimensions, document pages, and parser depth where applicable.

## AI properties

- Bound input bytes or tokens before the provider call.
- Set request deadlines and output-token caps.
- Bound concurrent executions and total automatic attempts.
- Count transcription, structuring, fallbacks, tool calls, and retries separately.
- Use structured output validation and treat user content as untrusted data.
- Configure provider or gateway alerts and a hard spend ceiling when available.

## Complete example: offline AI report

Flow: mobile recording → API record → signed object upload → confirmation → queue → transcription → structuring.

Required control plan:

1. Validate role and tenant at every API and object-key boundary.
2. Apply transport quotas to every request. Consume new-report actor, tenant, and global admission quotas all-or-nothing; a proven create or confirm replay skips only duplicate business and downstream charges.
3. Sign the real upload size and reject a bounded read beyond the maximum.
4. Consume the processing quota before enqueueing. Count manual retry against both retry and processing budgets.
5. Bound worker attempts, transcription duration, structuring tokens, and provider spend.
6. On `429`, keep the local report queued, preserve its permanent-failure count, and retry after bounded server guidance with jitter. After the automatic transient budget, pause for explicit resumable recovery without deleting the report.
7. If the limiter is unavailable, reject costly admission temporarily; do not discard the local report.
8. Clean abandoned uploads and alert on quota saturation, queue age, failed jobs, and spend.
