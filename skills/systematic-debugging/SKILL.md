---
name: systematic-debugging
description: Use when diagnosing a bug, test failure, regression, intermittent incident, or other unexpected software behavior before selecting a durable fix.
---

# Systematic Debugging

Find the earliest evidenced divergence between expected and actual behavior. Treat every proposed cause or fix as a hypothesis, regardless of who suggested it.

## Scope

- A diagnosis request authorizes inspection and reporting, not implementation.
- A fix request authorizes the smallest in-scope correction and relevant verification.
- During an active incident, separate reversible impact mitigation from root-cause correction. State what the mitigation proves and what remains unknown.

## Evidence loop

1. **Define the symptom.** Capture the exact failure, affected path, timing, frequency, environment, and last known working state. Preserve logs, traces, inputs, and exit codes that distinguish this failure from nearby noise.
2. **Locate the divergence.** Reproduce the symptom when practical. Otherwise trace one failing case across component boundaries and compare it with a working case. When edits are authorized, add temporary instrumentation only where evidence is missing; for read-only diagnosis, propose the exact instrumentation instead.
3. **Test one hypothesis.** State the suspected cause and the observation that would falsify it. Change one variable or run one focused probe. Record the result before forming the next hypothesis.
4. **Explain the cause.** Connect the triggering condition to the earliest incorrect state and then to the user-visible symptom. If that chain is incomplete, report the uncertainty instead of promoting a guess to root cause.
5. **Correct and prove.** When implementation is authorized, add the narrowest regression proof that fails on the original behavior, apply the smallest durable fix, and run the relevant focused and broader checks.

## Decision rules

- Passing local tests do not disprove an intermittent, environmental, concurrency, or production-only failure.
- Retries, rollbacks, cache bypasses, feature flags, and locks may be valid mitigations; they are not root-cause evidence by themselves.
- After several falsified hypotheses with no new evidence, widen observation at the next system boundary or name the missing production access or data needed to continue.
- Report the observed facts, eliminated hypotheses, remaining uncertainty, mitigation state, and next decisive probe.
