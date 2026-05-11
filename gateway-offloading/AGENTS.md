# Gateway Offloading Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Offloading pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading

## Pattern Boundary
Use this pattern when shared or specialized service functionality should be handled by a gateway instead of repeated in every backend service.

The gateway owns cross-cutting edge responsibilities such as TLS termination, authentication prechecks, request filtering, compression, caching, rate enforcement, or protocol handling.

## Required Shape
- Identify the offloaded responsibility and why it belongs at the gateway.
- Keep backend services simpler by removing duplicated edge concern code.
- Apply centralized policy consistently before traffic reaches services.
- Preserve backend visibility into identity, correlation, and request context.
- Test that offloaded behavior occurs at the gateway boundary.

## Do Not Build
- Do not duplicate the same offloaded concern inside every backend service.
- Do not put core domain decisions in the gateway.
- Do not use this folder for frontend-specific BFF behavior unless offloading remains the main pattern.
- Do not obscure who made security or traffic policy decisions.

## Review Checklist
- The gateway clearly performs a reusable cross-cutting responsibility.
- Backends no longer need to implement that same responsibility individually.
- Security and observability context survives the gateway hop.
