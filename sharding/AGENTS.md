# Sharding Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sharding pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding

## Pattern Boundary
Use this pattern when a data store must be horizontally partitioned across multiple shards to support scale, availability, tenant isolation, or operational boundaries.

Each shard stores a subset of data. Routing logic maps a request to the correct shard.

## Required Shape
- Define the shard key and the reason it distributes data effectively.
- Implement shard routing, lookup, or directory behavior.
- Keep per-shard schema, migrations, backups, and monitoring manageable.
- Define rebalancing, tenant movement, and hot-shard mitigation.
- Test single-shard operations and cross-shard edge cases.

## Do Not Build
- Do not call vertical table splitting or indexing sharding.
- Do not rely on cross-shard transactions as the normal write path.
- Do not choose a shard key that creates predictable hot shards without mitigation.
- Do not hardcode shard locations in business logic.

## Review Checklist
- Data placement is deterministic and observable.
- The system can add, remove, or rebalance shards deliberately.
- Operational tooling is shard-aware.
