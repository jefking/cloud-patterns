# Sequential Convoy Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sequential Convoy pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/sequential-convoy

## Pattern Boundary
Use this pattern when related messages must be processed in order while unrelated message groups can still process concurrently.

Ordering applies within a convoy group, session, partition, or correlation key. It should not force global serialization unless that is the actual business rule.

## Required Shape
- Define the grouping key that requires ordered processing.
- Use sessions, partitions, ordered queues, locks, or equivalent mechanisms to preserve per-group order.
- Allow different groups to process in parallel when safe.
- Handle stuck, poison, missing, or delayed messages without blocking all groups.
- Test ordering within a group and parallelism across groups.

## Do Not Build
- Do not process same-group messages concurrently when order matters.
- Do not serialize all work globally unless every message belongs to the same ordered group.
- Do not ignore poison messages that block a group indefinitely.
- Do not rely on accidental broker ordering without an explicit key or session rule.

## Review Checklist
- Ordered processing is guaranteed for each group.
- Unrelated groups can continue when one group is slow or failed.
- Blocking and recovery behavior for a convoy is defined.
