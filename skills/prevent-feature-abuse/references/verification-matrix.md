# Verification Matrix

Write tests before production changes. Each test must fail for the expected missing protection, then pass after the minimal control is added.

## Required tests by control

| Control | Minimum behavior tests |
|---|---|
| Actor quota | The narrowest accountable actor reaches its own ceiling; shared-IP users are not incorrectly merged when a stronger identity exists |
| Tenant/global quota | Different actors share the tenant or global ceiling; other tenants remain isolated |
| Atomicity | Actor, tenant, and global ceilings are consumed in one all-or-nothing decision; exhaustion, outage, or ambiguous response cannot partially debit any scope |
| Idempotency | Same actor, tenant, route, key, and payload replay without duplicate business work; every replay still consumes applicable transport, byte, concurrency, authentication, and response quotas |
| Payload bound | Exact maximum succeeds; maximum + 1 fails before full read or processing |
| Signed upload | Declared-small/actual-large, altered header, missing header, and reuse attempts fail |
| Queue admission | Duplicate event creates one logical job; queue failure does not create partial state |
| Retry budget | Automatic transient retries stop at the configured attempt or time horizon, enter a resumable paused state, and remain separate from permanent-failure attempts |
| Limiter outage | Costly work returns temporary failure and no provider call or irreversible effect occurs |
| Durable ingress | Limiter outage cannot exceed independently enforced item, byte, age, and capacity ceilings before costly processing |
| Offline client | `429` and temporary `503` preserve data, honor bounded retry guidance, do not consume permanent-failure attempts, and do not retry automatically forever |
| AI bounds | Timeout, output-token cap, schema rejection, and untrusted input boundaries reach the provider configuration |
| Retention | Abandoned and completed states follow their distinct cleanup policies |

## Verification sequence

1. Name the production behavior that will make the test pass.
2. Run the focused test and confirm the expected failure.
3. Implement the smallest complete protection.
4. Re-run the focused test.
5. Run adjacent feature tests, type checking, linting, and dependency audit as applicable.
6. Inspect the final diff and configuration values.

Do not claim the feature is abuse-proof. State which attack paths are bounded, their configured ceilings, and which operational controls remain external.

## Finding format

```text
[Severity] Short title
Attack path: prerequisite → action → amplified resource or impact
Evidence: file:line and current behavior
Maximum exposure: calculation or "not evidenced"
Control: authoritative boundary and exact evidenced limit, or unresolved limit variable
Test: red-green scenario proving the control
Residual risk: what remains and who owns it
```

## Completion gate

Before reporting completion, verify all applicable statements:

- Every costly or irreversible path has an admission bound.
- Every multi-scope admission is all-or-nothing; rejection or unavailability leaves every counter unchanged.
- Every replay remains bounded by the transport resources it consumes while duplicate business effects are free.
- Every payload has a real-resource bound, not only schema metadata.
- Every retry layer appears in the exposure equation.
- Rate-limit exhaustion and limiter outage have different tested behavior.
- Durable and offline clients preserve user data while waiting, then pause safely when automatic transient retries are exhausted.
- Every applicable actor, tenant, and global scope is tested; shared-network behavior is explicit.
- AI calls have time, output, concurrency, retry, and spend controls.
- Cleanup covers abandoned states as well as successful states.
- Fresh tests, type checks, and lint results support the final claims.
