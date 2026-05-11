# Valet Key Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Valet Key pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/valet-key

## Pattern Boundary
Use this pattern when a service should grant a client limited direct access to a resource without proxying all resource traffic through the service.

The service issues a scoped, time-limited token or key. The client uses it directly against the resource, and the resource enforces the constraints.

## Required Shape
- Identify the broker service, client, target resource, and delegated operation.
- Generate credentials with least privilege, narrow scope, and short lifetime.
- Bind permissions to the intended resource, action, and tenant or user context.
- Define expiration, revocation, auditing, and replay protection where needed.
- Test successful delegated access and denied access outside the token scope.

## Do Not Build
- Do not give clients master keys or broad service credentials.
- Do not route all large data transfer through the broker when direct scoped access is intended.
- Do not issue indefinite or reusable credentials without business justification.
- Do not trust client claims that are not encoded or enforced in the delegated credential.

## Review Checklist
- Delegated access is direct, scoped, and time-bounded.
- The broker never exposes privileged credentials to clients.
- Unauthorized actions fail at the resource boundary.
