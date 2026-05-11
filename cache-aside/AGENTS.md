# Cache-Aside Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Cache-Aside pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside

## Pattern Boundary
Use this pattern when application code manages a cache beside a primary data store. The application checks the cache first, loads missing data from the store, then populates the cache for later reads.

The primary store remains authoritative. The cache is a performance optimization with explicit expiration and invalidation behavior.

## Required Shape
- Implement read-through behavior in application code: check cache, load store on miss, then populate cache.
- Keep the source data store as the authority for writes and consistency.
- Define TTL, invalidation, refresh, or eviction rules for cached entries.
- Handle cache misses, stale entries, stampedes, and cache service failures.
- Test both cache-hit and cache-miss paths.

## Do Not Build
- Do not treat the cache as the only durable source of data.
- Do not update cached data without a clear strategy for the backing store.
- Do not ignore concurrent cache misses for expensive values.
- Do not use this folder for generic CDN or static asset hosting.

## Review Checklist
- Cache keys, TTLs, and invalidation rules are documented in code or tests.
- The system still works when the cache is empty or unavailable.
- Writes cannot leave the cache and backing store silently inconsistent.
