# Backends for Frontends Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Backends for Frontends pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends

## Pattern Boundary
Use this pattern when different frontend applications or client interfaces need backend APIs shaped specifically for their workflows, payloads, latency needs, and security context.

Each frontend gets a dedicated backend surface. Shared domain services can remain behind those backends, but the frontend-specific backend owns client composition and client policy.

## Required Shape
- Identify each frontend and its dedicated backend.
- Shape payloads, authentication flow, and API ergonomics around that frontend.
- Keep frontend-specific aggregation, filtering, and protocol choices in the BFF.
- Keep shared business rules in shared services or libraries when they are not frontend-specific.
- Test each BFF against the needs and constraints of its frontend.

## Do Not Build
- Do not create one generic backend that every frontend must adapt to.
- Do not let frontends call many internal services directly when the BFF should compose those calls.
- Do not duplicate core business rules across BFFs without a deliberate reason.
- Do not use the BFF as an unrestricted gateway for unrelated clients.

## Review Checklist
- Each frontend has a clearly owned backend contract.
- The BFF reduces frontend complexity rather than exposing internal service topology.
- Shared logic and frontend-specific logic are separated intentionally.
