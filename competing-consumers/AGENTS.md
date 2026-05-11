# Competing Consumers Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Competing Consumers pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/competing-consumers

## Pattern Boundary
Use this pattern when multiple worker instances should pull from the same queue or channel so work can be processed concurrently and scaled horizontally.

Each message should be handled by one consumer instance, with retries and idempotency protecting against duplicate delivery.

## Required Shape
- Use a shared queue, work channel, or equivalent competing-consumer source.
- Run multiple interchangeable consumers against that source.
- Define message lock, acknowledgement, retry, and dead-letter behavior.
- Make message handling idempotent or otherwise duplicate-safe.
- Add scale and failure tests with more than one consumer.

## Do Not Build
- Do not use publish-subscribe fan-out when each work item should be processed once.
- Do not depend on global ordering unless the design includes partitioned ordering rules.
- Do not let consumers share unsafe mutable state.
- Do not acknowledge work before it is durably completed.

## Review Checklist
- Consumers can scale out without changing the producer.
- A failed consumer does not permanently lose a message.
- Duplicate delivery and poison messages are handled explicitly.
