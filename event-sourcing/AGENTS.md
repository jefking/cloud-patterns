# Event Sourcing Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Event Sourcing pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing

## Pattern Boundary
Use this pattern when state must be captured as an append-only sequence of domain events from which current state and read projections are derived.

The event log is the durable source of truth. Current state is reconstructed from events or snapshots.

## Required Shape
- Persist immutable domain events in append-only order per aggregate or stream.
- Rehydrate domain state from events and optional snapshots.
- Use commands to validate intent before appending new events.
- Version event schemas and provide upcasters or compatibility handling.
- Build projections as derived read models that can be rebuilt.

## Do Not Build
- Do not treat a mutable current-state table as the primary source of truth.
- Do not rewrite, delete, or reorder events casually.
- Do not store vague change logs that cannot rebuild state.
- Do not let projections become the only durable record of business facts.

## Review Checklist
- Events describe business facts, not implementation steps.
- State can be rebuilt from the event stream.
- Schema evolution, replay, and projection rebuild behavior are covered.
