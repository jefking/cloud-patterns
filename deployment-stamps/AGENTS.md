# Deployment Stamps Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Deployment Stamps pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/deployment-stamp

## Pattern Boundary
Use this pattern when a workload needs multiple independent copies of the same application components, often per region, tenant group, scale unit, or isolation boundary.

A stamp is a repeatable deployment unit with its own compute, data, configuration, and lifecycle.

## Required Shape
- Define the stamp template and the resources included in each stamp.
- Provide routing or placement logic that maps requests, tenants, or regions to stamps.
- Keep per-stamp data and dependencies independent unless cross-stamp sharing is explicit.
- Automate stamp provisioning, upgrades, removal, and health checks.
- Test at least two stamps to prove repeatability and isolation.

## Do Not Build
- Do not make one bespoke deployment and call it a stamp.
- Do not create hidden central dependencies that make all stamps fail together.
- Do not require manual drift-prone changes per stamp.
- Do not share tenant state across stamps without a migration and consistency plan.

## Review Checklist
- A new stamp can be created from the same definition.
- Stamps can fail, scale, and roll out independently.
- Routing and operational visibility are stamp-aware.
