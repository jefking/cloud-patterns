# Retry Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Retry pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/retry

## Pattern Boundary
Use this pattern when operations can fail due to transient faults and a later attempt is likely to succeed.

Retries must be bounded, delay-aware, and safe for the operation being retried.

## Required Shape
- Identify which errors are transient and which are terminal.
- Use bounded attempts with exponential backoff, jitter, or provider-directed delay.
- Respect operation timeouts, cancellation, and caller deadlines.
- Make retried operations idempotent or protect them with idempotency keys.
- Emit telemetry for attempts, final outcome, and exhausted retries.

## Do Not Build
- Do not retry validation errors, authorization failures, or deterministic business failures.
- Do not retry forever.
- Do not retry non-idempotent operations without duplicate protection.
- Do not stack retries across layers without an overall budget.

## Review Checklist
- Retry policy is specific to the dependency and failure type.
- The operation remains safe if the first attempt succeeded but the response was lost.
- Exhausted retries produce a clear failure path.
