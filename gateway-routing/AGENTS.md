# Gateway Routing Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Routing pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing

## Pattern Boundary
Use this pattern when clients should use one endpoint while the gateway routes requests to multiple backend services.

The gateway selects the backend by route, host, version, tenant, capability, or policy. It does not primarily aggregate responses.

## Required Shape
- Define the public endpoint and route table or routing policy.
- Map routes to backend services without exposing internal service addresses to clients.
- Include health-aware routing, versioning, and failure behavior where relevant.
- Preserve correlation, identity, and request metadata across the routed hop.
- Test route selection, missing routes, backend failure, and version or tenant routing.

## Do Not Build
- Do not require clients to know internal backend locations.
- Do not aggregate multiple backend responses as the main behavior.
- Do not put domain workflow decisions in the gateway.
- Do not route to unhealthy backends when health data is available.

## Review Checklist
- One client-facing endpoint can reach multiple services through explicit routes.
- Routing behavior is observable and testable.
- Backend topology can change without breaking client contracts.
