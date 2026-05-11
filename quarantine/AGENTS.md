# Quarantine Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Quarantine pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/quarantine

## Pattern Boundary
Use this pattern when externally supplied assets must be isolated until they meet quality, security, policy, or validation requirements.

Assets enter a quarantined area first. Only validated assets are promoted to trusted storage or made available to the workload.

## Required Shape
- Separate quarantine storage or state from trusted storage or state.
- Define validation, scanning, approval, and rejection rules.
- Prevent workload consumption before promotion.
- Record audit evidence for validation and promotion decisions.
- Handle expired, failed, malicious, or manually reviewed assets.

## Do Not Build
- Do not allow unvalidated external assets into trusted runtime paths.
- Do not let manual bypasses skip audit and policy checks.
- Do not mix quarantined and trusted assets in the same indistinguishable location.
- Do not treat a filename or source claim as proof of trust.

## Review Checklist
- Every external asset starts untrusted.
- Promotion requires explicit successful validation.
- Rejected and expired assets are contained and auditable.
