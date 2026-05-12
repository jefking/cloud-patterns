# Deployment Stamps Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Deployment Stamps pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/deployment-stamp
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Deploy multiple repeatable, independent copies of application components as scale units, regions, or tenant cells.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The workload needs repeatable isolation by tenant group, geography, capacity unit, environment, or blast radius.
- A stamp can include compute, data stores, networking, configuration, and operational controls.
- Routing or placement can assign users, tenants, or traffic to a stamp.

Do not use this pattern when:
- There is only one bespoke deployment with no repeatable template.
- Hidden shared dependencies make all stamps fail together.
- Manual per-stamp drift is required for normal operation.

## Architecture Invariants
- A stamp is created from a common definition and can be repeated predictably.
- Each stamp owns its runtime resources and failure domain.
- Routing, tenant placement, and migration logic are stamp-aware.
- Operations such as deploy, upgrade, scale, backup, and remove can run per stamp.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Automate provisioning and configuration to prevent drift.
- Expose per-stamp health, capacity, cost, version, and tenant distribution.
- Plan data residency, cross-stamp migration, and noisy-neighbor isolation.
- Test at least two stamps and a failed or drained stamp route.

## Pattern Boundaries and Common Confusions
- Geode is active-active geographic operation; Deployment Stamps are repeatable deployment units that may or may not be geographic.
- Bulkhead isolates resources; stamps duplicate a broader application slice.
- Sharding partitions data; stamps partition a deployable workload unit.

## Verification Checklist
- Can a new stamp be created without bespoke manual steps?
- Can one stamp fail without taking all tenants down?
- Can traffic be placed, moved, or drained by stamp?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
