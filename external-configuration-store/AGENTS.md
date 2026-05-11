# External Configuration Store Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the External Configuration Store pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/external-configuration-store

## Pattern Boundary
Use this pattern when configuration should live outside the application deployment package so it can be managed centrally across environments and instances.

The application reads configuration from a managed external source with validation, access control, and refresh behavior.

## Required Shape
- Move environment-specific configuration out of packaged code and static binaries.
- Load configuration from a centralized service, file store, database, or platform configuration system.
- Validate configuration shape, required keys, and allowed values at startup or refresh.
- Separate secrets from general configuration and use an appropriate secret store.
- Define refresh, caching, fallback, and audit behavior.

## Do Not Build
- Do not hardcode environment values in application code.
- Do not require redeployment for routine configuration changes unless immutability is explicit.
- Do not store secrets as plain configuration values.
- Do not let missing or malformed configuration fail unpredictably at runtime.

## Review Checklist
- Configuration source, precedence, and refresh rules are clear.
- Configuration access is authenticated and least-privilege.
- Invalid configuration fails safely and observably.
