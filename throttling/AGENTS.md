# Throttling Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Throttling pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/throttling

## Pattern Boundary
Use this pattern when a service must protect itself by controlling how much resource consumption callers, tenants, users, or workloads can impose.

Throttling is service-side demand control. It enforces fairness, preserves availability, and communicates limits to callers.

## Required Shape
- Define quotas, limits, or concurrency caps by caller, tenant, resource, or operation.
- Enforce limits before protected resources are exhausted.
- Return explicit throttling responses and retry guidance where appropriate.
- Include metrics for allowed, delayed, rejected, and throttled requests.
- Test over-limit, burst, recovery, and fairness behavior.

## Do Not Build
- Do not rely only on clients to self-limit traffic.
- Do not silently drop work without a documented contract.
- Do not use one undifferentiated limit when tenants or operations need different policies.
- Do not throttle after expensive protected work has already run.

## Review Checklist
- The service protects itself before overload becomes failure.
- Callers receive clear signals about throttling and recovery.
- Limits are observable, fair, and aligned to business policy.
