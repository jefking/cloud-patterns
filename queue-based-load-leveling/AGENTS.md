# Queue-Based Load Leveling Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Queue-Based Load Leveling pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling

## Pattern Boundary
Use this pattern when a queue should buffer bursts of work between producers and consumers so downstream services can process at a sustainable rate.

The queue absorbs intermittent load. Workers drain it according to capacity, scale rules, and backpressure policy.

## Required Shape
- Put a durable queue or equivalent buffer between producers and consumers.
- Define producer enqueue behavior and consumer processing rate.
- Add retry, visibility timeout, dead-letter, and poison-message handling.
- Expose queue depth, age, throughput, and failure metrics.
- Test burst load and downstream slowdown scenarios.

## Do Not Build
- Do not make producers synchronously call overloaded consumers for bursty work.
- Do not use this pattern when the caller requires immediate completion.
- Do not ignore queue growth, message age, or poison messages.
- Do not acknowledge work before processing is durably complete.

## Review Checklist
- The queue smooths demand instead of moving the bottleneck invisibly.
- Consumer capacity and scaling are tied to queue health.
- Backpressure and failure behavior are explicit.
