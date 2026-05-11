# Rate Limiting Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Rate Limiting pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern

## Pattern Boundary
Use this pattern when a workload must control its own consumption of a resource or dependency to avoid throttling errors, quota exhaustion, or cascading retries.

Rate limiting here is consumer-side or caller-side demand shaping. It protects the workload from exceeding provider limits.

## Required Shape
- Identify the constrained dependency, quota, or provider limit.
- Enforce call rate, concurrency, burst, or token budgets before requests are sent.
- Honor provider guidance such as retry-after values when available.
- Queue, shed, or defer excess work according to business priority.
- Add metrics for allowed, delayed, rejected, and retried calls.

## Do Not Build
- Do not send unlimited traffic and rely on the provider to throttle.
- Do not create retry storms after rate-limit responses.
- Do not use this folder for service-side throttling unless caller-side rate limiting remains central.
- Do not apply one global limit when different dependencies have different quotas.

## Review Checklist
- The caller has explicit budgets before it consumes the dependency.
- Backoff and retry behavior respects quota recovery.
- Operators can see when rate limiting is protecting the system.
