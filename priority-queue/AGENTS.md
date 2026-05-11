# Priority Queue Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Priority Queue pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/priority-queue

## Pattern Boundary
Use this pattern when some queued work should be processed sooner than other work based on business or operational priority.

Priority must be explicit and enforced by the queue topology, message metadata, or consumer scheduling policy.

## Required Shape
- Define priority classes and the rule that assigns each work item to a class.
- Implement priority-aware enqueueing and dequeueing.
- Prevent starvation through aging, quotas, or reserved capacity when lower priorities still matter.
- Add metrics for queue depth, wait time, and processing by priority.
- Test that high-priority work is processed ahead of lower-priority work under load.

## Do Not Build
- Do not use a single FIFO queue with no priority handling.
- Do not allow priority to bypass security, tenancy, or entitlement rules.
- Do not starve lower-priority work unless the business rule explicitly allows it.
- Do not let producers set arbitrary priority without validation.

## Review Checklist
- Priority assignment is deterministic and auditable.
- Consumers demonstrably respect priority under contention.
- Starvation and fairness behavior are deliberate.
