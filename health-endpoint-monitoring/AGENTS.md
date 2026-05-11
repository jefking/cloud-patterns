# Health Endpoint Monitoring Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Health Endpoint Monitoring pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring

## Pattern Boundary
Use this pattern when external tools need endpoints that report whether an application or service is functioning correctly.

Health endpoints should exercise meaningful checks and distinguish operational states that automation can act on.

## Required Shape
- Expose one or more health endpoints for monitoring systems.
- Separate liveness, readiness, startup, and dependency health when those states differ.
- Check critical dependencies or workflows, not only process existence.
- Secure endpoints and avoid leaking sensitive details.
- Emit metrics or logs that explain health state changes.

## Do Not Build
- Do not rely only on a shallow ping for critical workload readiness.
- Do not expose secrets, connection strings, or detailed internals in health responses.
- Do not make health checks so expensive that monitoring creates load problems.
- Do not report healthy while required dependencies are unavailable.

## Review Checklist
- Automation can safely act on each health status.
- Health checks cover the dependencies that matter for the advertised state.
- Failure responses are safe, concise, and observable.
