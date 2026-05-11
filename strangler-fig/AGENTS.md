# Strangler Fig Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Strangler Fig pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig

## Pattern Boundary
Use this pattern when a legacy system should be replaced incrementally by routing selected capabilities to new services while the legacy system continues to run.

A facade, router, or interception layer directs traffic to legacy or new implementations during migration.

## Required Shape
- Identify the legacy capability being wrapped and the new replacement capability.
- Add a facade, route, proxy, or interception layer that controls cutover.
- Migrate one capability or slice at a time with fallback and rollback options.
- Manage data ownership, synchronization, and source-of-truth transitions explicitly.
- Test legacy route, new route, rollback, and coexistence behavior.

## Do Not Build
- Do not perform a big-bang rewrite in this folder.
- Do not bypass the facade for migrated capabilities.
- Do not leave duplicate sources of truth without reconciliation rules.
- Do not migrate unrelated functionality in the same slice without a routing reason.

## Review Checklist
- Traffic can be shifted gradually from legacy to new behavior.
- Coexistence and rollback are safe.
- Each migrated slice has clear ownership and data consistency rules.
