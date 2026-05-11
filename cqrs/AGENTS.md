# CQRS Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the CQRS pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs

## Pattern Boundary
Use this pattern when read operations and write operations need different models, interfaces, scaling behavior, or consistency rules.

Commands change state through validated write paths. Queries read from models optimized for retrieval and must not mutate business state.

## Required Shape
- Separate command interfaces from query interfaces.
- Keep write models responsible for invariants, validation, and state changes.
- Keep read models optimized for query needs and explicitly synchronized from writes.
- Define consistency expectations between command completion and query visibility.
- Test commands and queries independently.

## Do Not Build
- Do not expose a single CRUD model and call it CQRS.
- Do not let query handlers mutate domain state.
- Do not bypass command validation by writing directly to read stores.
- Do not introduce separate stores unless the problem justifies the added consistency cost.

## Review Checklist
- Command and query responsibilities are visibly separate.
- Read models can lag behind writes only within documented rules.
- Tests cover invariants on the command side and projection behavior on the query side.
