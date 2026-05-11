# Compensating Transaction Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Compensating Transaction pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction

## Pattern Boundary
Use this pattern when a multi-step operation spans resources that cannot participate in a single atomic transaction. Failed or canceled work is corrected through explicit compensating actions.

The system accepts eventual consistency and records enough progress to undo, amend, or counteract completed steps.

## Required Shape
- Identify each forward step and its corresponding compensation.
- Persist workflow progress so recovery can resume compensation after failure.
- Make compensating actions idempotent and safe to retry.
- Define ordering rules for compensation based on business dependencies.
- Test partial failure at every step.

## Do Not Build
- Do not pretend a distributed operation is ACID when the resources cannot support it.
- Do not omit compensation for irreversible or externally visible steps without documenting the business decision.
- Do not rely only on in-memory state to know what must be compensated.
- Do not assume compensation is always strict reverse order.

## Review Checklist
- Every committed step has a deliberate recovery or compensation path.
- Compensation can survive process restarts and duplicate messages.
- Final states are observable and reconcilable.
