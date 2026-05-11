# Ambassador Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Ambassador pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador

## Pattern Boundary
Use this pattern when a consumer delegates outbound network communication to a helper service, proxy, or companion component.

The consumer owns business behavior. The ambassador owns remote connectivity concerns such as service discovery, authentication to downstream services, retries, timeouts, telemetry, protocol adaptation, and connection pooling.

## Required Shape
- Identify the consumer, the ambassador, and the downstream service.
- Route outbound calls from the consumer through the ambassador boundary.
- Keep network policy and cross-cutting communication behavior in the ambassador, not in the consumer.
- Make ambassador configuration explicit, observable, and independently testable.
- Provide health checks, timeout behavior, and failure reporting at the ambassador boundary.

## Do Not Build
- Do not implement a general API gateway for many unrelated consumers.
- Do not put domain business logic in the ambassador.
- Do not let the consumer bypass the ambassador for the same downstream dependency.
- Do not mix in sibling patterns unless the task explicitly asks for integration and the ambassador remains the controlling pattern.

## Review Checklist
- The request path through the ambassador is clear.
- Tests or examples prove that direct downstream access is not required by the consumer.
- Failure, retry, and observability behavior are owned by the ambassador component.
