# Federated Identity Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Federated Identity pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/federated-identity

## Pattern Boundary
Use this pattern when authentication should be delegated to an external identity provider instead of handled directly by the application.

The application trusts validated tokens or assertions from the identity provider and maps identity claims to local authorization decisions.

## Required Shape
- Identify the identity provider, relying party, token format, and trust relationship.
- Validate issuer, audience, signature, expiry, and required claims.
- Map external claims to local roles, permissions, or tenant context.
- Keep authorization decisions explicit and separate from authentication.
- Test login, token validation failure, claim mapping, and logout or session expiry.

## Do Not Build
- Do not store or verify user passwords locally when the IdP owns authentication.
- Do not accept unsigned, expired, wrong-audience, or untrusted tokens.
- Do not treat identity claims as authorization without local policy.
- Do not mix multiple IdPs without clear trust and claim normalization rules.

## Review Checklist
- Authentication is delegated and token validation is strict.
- Local authorization is based on normalized, trusted identity data.
- Failure modes do not create anonymous or over-privileged access.
