# Anti-Corruption Layer Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Anti-Corruption Layer pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Protect a clean domain model from a legacy, external, or incompatible model by translating at an explicit boundary.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Two systems must integrate but use different domain language, schemas, identifiers, protocols, or error semantics.
- The local model must remain independent of the external or legacy model.
- A translation layer can own mapping, validation, and compatibility behavior.

Do not use this pattern when:
- The proposed code lets legacy DTOs, tables, status codes, or naming leak into the protected domain.
- The task is a full replacement of the legacy system rather than boundary translation.
- Both sides already share the same model and no semantic protection is needed.

## Architecture Invariants
- The protected domain speaks only its own language at its public and internal boundaries.
- Adapters map requests, responses, events, errors, and identifiers explicitly.
- External contracts terminate at the anti-corruption layer and do not cross into domain code.
- Mapping rules are tested as first-class behavior, not treated as incidental serialization.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Version external contract mappings and handle backward compatibility deliberately.
- Normalize external failures into domain-appropriate errors without losing diagnostic detail.
- Add logging around translations, rejected inputs, and schema mismatches.
- Design for partial availability when the legacy or external system is slow or unavailable.

## Pattern Boundaries and Common Confusions
- A facade alone is not enough unless it prevents semantic leakage.
- A generic API client is not an anti-corruption layer unless it translates models and behavior.
- Strangler Fig migrates functionality over time; Anti-Corruption Layer protects a boundary during integration.

## Verification Checklist
- Can domain code be tested without importing external schemas or clients?
- Are all model and error translations covered by tests?
- Can the external system evolve without forcing local domain renames or structural changes?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
