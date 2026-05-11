# Circuit Breaker Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Circuit Breaker pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker

## Pattern Boundary
Use this pattern when calls to a remote dependency can fail for variable periods and repeated calls would worsen load, latency, or user experience.

The circuit breaker monitors failures, opens to fail fast, probes recovery, and closes after the dependency becomes healthy again.

## Required Shape
- Model closed, open, and half-open states or their operational equivalent.
- Define failure thresholds, reset timing, timeout behavior, and probe rules.
- Provide fallback, fail-fast, or degraded behavior for open circuits.
- Emit metrics and logs for state changes and rejected calls.
- Test transitions across success, failure, open, probe, and recovery paths.

## Do Not Build
- Do not retry indefinitely against an unhealthy dependency.
- Do not hide circuit-open behavior from callers and operators.
- Do not use circuit breaking for local validation or deterministic business errors.
- Do not share one circuit across unrelated dependencies with different failure profiles.

## Review Checklist
- State transitions are deterministic and covered by tests.
- Open circuits reduce pressure on the failing dependency.
- Recovery probes are bounded and observable.
