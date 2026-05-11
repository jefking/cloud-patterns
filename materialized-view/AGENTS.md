# Materialized View Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Materialized View pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/materialized-view

## Pattern Boundary
Use this pattern when reads need a precomputed view over data that is expensive, awkward, or slow to query directly from source stores.

The materialized view is derived data optimized for query access. Source stores remain authoritative.

## Required Shape
- Identify the read workload and the source data used to build the view.
- Define the projection shape, refresh trigger, and consistency expectations.
- Support rebuilding the view from authoritative sources.
- Handle partial refresh, failed refresh, and stale-view behavior.
- Test view generation and query behavior separately from source writes.

## Do Not Build
- Do not make the materialized view the only durable source of truth.
- Do not update the view manually as if it were primary state.
- Do not use this pattern when a normal index or query optimization is sufficient.
- Do not hide staleness from callers when it matters to behavior.

## Review Checklist
- The view is clearly derived and rebuildable.
- Refresh and consistency rules are explicit.
- Query performance improves for the intended read path.
