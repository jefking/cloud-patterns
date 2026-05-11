# Saga Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Saga pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/saga

## Pattern Boundary
Use this pattern when a business transaction spans multiple services and must be completed through a sequence of local transactions with compensation for failures.

A saga coordinates consistency across services without a single distributed database transaction. Coordination can be orchestrated or choreographed, but saga state and compensation remain explicit.

## Required Shape
- Identify each local transaction, participant, and compensation.
- Persist saga state, correlation identifiers, and terminal outcomes.
- Make participant commands and compensations idempotent.
- Define timeout, retry, failure, and manual intervention behavior.
- Test failures at every participant boundary.

## Do Not Build
- Do not use a distributed ACID transaction as the main solution.
- Do not share one database transaction across services.
- Do not leave completed local transactions without compensation or reconciliation.
- Do not lose saga progress when a coordinator or participant restarts.

## Review Checklist
- The saga can recover from partial completion.
- State transitions and compensation paths are explicit.
- Participants remain service-owned and locally transactional.
