# Static Content Hosting Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Static Content Hosting pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/static-content-hosting

## Pattern Boundary
Use this pattern when static assets should be served directly from cloud storage, object storage, or a CDN instead of through application compute.

Application servers generate or reference assets as needed, but delivery of static bytes is offloaded to the static hosting platform.

## Required Shape
- Identify the static assets and the storage or CDN endpoint that serves them.
- Keep static delivery outside application request processing.
- Define cache headers, versioning, invalidation, compression, and content type handling.
- Apply access control for private or restricted assets.
- Test direct asset delivery, cache behavior, and invalidation.

## Do Not Build
- Do not route routine static file traffic through application servers.
- Do not place dynamic business processing in the static hosting path.
- Do not expose private assets through public storage by accident.
- Do not ignore cache invalidation and asset versioning.

## Review Checklist
- Static assets are delivered by storage or CDN infrastructure.
- Application compute is not required for normal static reads.
- Cache and access behavior are deliberate and testable.
