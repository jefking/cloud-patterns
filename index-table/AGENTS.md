# Index Table Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Index Table pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/index-table

## Pattern Boundary
Use this pattern when query performance requires secondary lookup structures over fields that the primary data store does not index efficiently.

The index table maps query keys to primary records or denormalized lookup data. It is maintained as a derived structure.

## Required Shape
- Identify the query patterns that require an index table.
- Define index keys, stored values, and links back to primary records.
- Update the index when primary data changes, synchronously or through a reliable async process.
- Define consistency lag, rebuild, and repair behavior.
- Test queries through the index and updates that affect indexed fields.

## Do Not Build
- Do not scan the primary store for queries the index is meant to serve.
- Do not treat the index as the sole authoritative record.
- Do not create indexes unrelated to actual query requirements.
- Do not ignore orphaned or stale index entries.

## Review Checklist
- The index table exists to serve a specific query shape.
- Primary data and index data have a clear synchronization path.
- Rebuild or reconciliation behavior is available.
