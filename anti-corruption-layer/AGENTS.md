# Anti-Corruption Layer Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Anti-Corruption Layer pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer

## Pattern Boundary
Use this pattern when a modern application or bounded context must integrate with a legacy, external, or otherwise incompatible system without adopting that system's model.

The anti-corruption layer is a translation boundary. It protects the local domain model from foreign protocols, schemas, error formats, identifiers, and lifecycle assumptions.

## Required Shape
- Identify the protected domain and the external or legacy system.
- Place a facade, adapter, mapper, or translator between the two models.
- Translate commands, queries, events, errors, and identifiers explicitly at the boundary.
- Keep foreign DTOs and transport details out of core domain code.
- Add tests that prove model translation and error translation behavior.

## Do Not Build
- Do not let the local domain directly depend on external schemas or legacy data structures.
- Do not solve integration by rewriting the legacy system in this folder.
- Do not leak foreign status codes, persistence concepts, or naming conventions into domain APIs.
- Do not bypass the anti-corruption layer for convenience paths.

## Review Checklist
- The boundary API uses local domain language.
- The external integration is isolated behind adapters or facades.
- Translation failures are handled deliberately and do not corrupt local state.
