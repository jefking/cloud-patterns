# Geode Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Geode pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes

## Pattern Boundary
Use this pattern when a workload should run active service nodes across multiple geographic regions so users can be served by any nearby or available node.

Each geode can handle requests independently while data replication, partitioning, and conflict handling keep the system coherent.

## Required Shape
- Identify the geographic nodes and the routing strategy between them.
- Make each node capable of serving live traffic, not only standby traffic.
- Define data replication, partitioning, consistency, and conflict-resolution behavior.
- Keep regional health, failover, and traffic steering observable.
- Test regional loss, latency, and data convergence scenarios.

## Do Not Build
- Do not implement only active-passive disaster recovery.
- Do not require every request to depend on a single central region.
- Do not ignore write conflicts, replication lag, or jurisdictional data constraints.
- Do not hardcode region-specific behavior that prevents adding another geode.

## Review Checklist
- Multiple regions can actively serve the workload.
- Regional failure does not depend on a manual rewrite of routing or state.
- Data consistency tradeoffs are explicit and tested.
