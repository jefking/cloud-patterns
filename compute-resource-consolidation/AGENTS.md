# Compute Resource Consolidation Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Compute Resource Consolidation pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/compute-resource-consolidation

## Pattern Boundary
Use this pattern when multiple tasks or operations can safely share a compute unit to improve utilization, reduce overhead, or simplify deployment.

Consolidation is justified by compatible resource profiles and operational boundaries, not by convenience alone.

## Required Shape
- Identify the tasks being consolidated and why they can share compute safely.
- Define resource limits, scheduling rules, and failure isolation inside the shared compute unit.
- Preserve observability per task even when runtime is shared.
- Document deployment, scaling, and rollback impact of the consolidated unit.
- Test contention and failure behavior between co-located tasks.

## Do Not Build
- Do not consolidate tasks with incompatible CPU, memory, latency, or security requirements.
- Do not hide per-task failures inside a single coarse health signal.
- Do not combine deploy lifecycles when independent rollout is required.
- Do not turn consolidation into unrelated monolithic business coupling.

## Review Checklist
- Shared compute has a clear utilization or operational rationale.
- One task cannot silently starve the others.
- Operators can inspect and manage each task within the consolidated unit.
