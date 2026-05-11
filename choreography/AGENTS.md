# Choreography Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Choreography pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography

## Pattern Boundary
Use this pattern when services should coordinate a business process by reacting to events without a central orchestrator commanding every step.

Each participant owns its local decision to publish or react. The overall process emerges from event contracts and service responsibilities.

## Required Shape
- Identify participating services and the events they publish or consume.
- Make event contracts explicit and versionable.
- Keep each service responsible for its own local transaction and reaction logic.
- Make handlers idempotent and resilient to duplicate or out-of-order events.
- Provide traceability for the end-to-end process through correlation identifiers.

## Do Not Build
- Do not introduce a central workflow controller that tells every participant what to do.
- Do not coordinate participants through a shared database.
- Do not create synchronous call chains and label them choreography.
- Do not let publishers depend on specific subscriber implementations.

## Review Checklist
- The business process can be understood from events and local reactions.
- Services remain loosely coupled through event contracts.
- Duplicate, missing, and late events have deliberate handling.
