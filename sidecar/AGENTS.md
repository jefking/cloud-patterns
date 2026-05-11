# Sidecar Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sidecar pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar

## Pattern Boundary
Use this pattern when a supporting component should run beside an application process or container to provide capabilities without being built into the application.

The sidecar shares the application's lifecycle and local environment while remaining separately packaged or configured.

## Required Shape
- Identify the primary application and its sidecar component.
- Keep the sidecar focused on supporting capabilities such as proxying, logging, configuration, security, or synchronization.
- Define local communication, lifecycle coupling, resource limits, and startup ordering.
- Keep business behavior in the primary application.
- Test primary app behavior with the sidecar present, absent, and failing.

## Do Not Build
- Do not turn the sidecar into a shared centralized service for unrelated applications.
- Do not put core domain logic in the sidecar.
- Do not require application code to know remote topology that the sidecar is supposed to abstract.
- Do not hide sidecar failures from health and deployment checks.

## Review Checklist
- The sidecar is co-located and lifecycle-coupled with the primary app.
- Responsibilities are separated between app and sidecar.
- Failure and resource behavior are visible at the pod, process, or host boundary.
