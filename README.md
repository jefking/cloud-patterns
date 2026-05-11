# cloud-patterns

Pattern-scoped agent instructions for cloud architecture work.

This repository contains one folder for each Azure Architecture Center cloud
design pattern. Each folder contains an `AGENTS.md` file that constrains agents
working in that folder to design and build software using that pattern only.

The pattern list is based on the Microsoft Learn catalog:
https://learn.microsoft.com/en-us/azure/architecture/patterns/

## Purpose

Use this repo as a guardrail library for agent-driven development. When an
agent is placed in a pattern folder, the local `AGENTS.md` tells it:

- what the pattern is for
- what implementation shape is required
- what architectural drift to avoid
- what review checks must pass before work is considered complete

The files are intentionally directive. They are meant to shape implementation
behavior, not to replace the full Microsoft Learn documentation.

## Repository Layout

```text
cloud-patterns/
  README.md
  LICENSE
  <pattern-slug>/
    AGENTS.md
```

Each pattern folder is named with a stable, lowercase, kebab-case slug. For
example:

```text
cache-aside/AGENTS.md
saga/AGENTS.md
sidecar/AGENTS.md
```

## How To Use

1. Choose the pattern that should govern the work.
2. Put the implementation, example, test, or task context inside that pattern's
   folder.
3. Run the agent from that folder, or make sure the folder's `AGENTS.md` is in
   scope for the agent.
4. Review the result against the `Review Checklist` section in that
   `AGENTS.md`.

The intent is that work inside `retry/` stays focused on the Retry pattern,
work inside `saga/` stays focused on the Saga pattern, and so on.

## Pattern Catalog

| Pattern | Local instructions | Microsoft Learn |
| --- | --- | --- |
| Ambassador | [ambassador/AGENTS.md](ambassador/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador) |
| Anti-Corruption Layer | [anti-corruption-layer/AGENTS.md](anti-corruption-layer/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer) |
| Asynchronous Request-Reply | [asynchronous-request-reply/AGENTS.md](asynchronous-request-reply/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/asynchronous-request-reply) |
| Backends for Frontends | [backends-for-frontends/AGENTS.md](backends-for-frontends/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends) |
| Bulkhead | [bulkhead/AGENTS.md](bulkhead/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead) |
| Cache-Aside | [cache-aside/AGENTS.md](cache-aside/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside) |
| Choreography | [choreography/AGENTS.md](choreography/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography) |
| Circuit Breaker | [circuit-breaker/AGENTS.md](circuit-breaker/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) |
| Claim Check | [claim-check/AGENTS.md](claim-check/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/claim-check) |
| Compensating Transaction | [compensating-transaction/AGENTS.md](compensating-transaction/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction) |
| Competing Consumers | [competing-consumers/AGENTS.md](competing-consumers/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/competing-consumers) |
| Compute Resource Consolidation | [compute-resource-consolidation/AGENTS.md](compute-resource-consolidation/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/compute-resource-consolidation) |
| CQRS | [cqrs/AGENTS.md](cqrs/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs) |
| Deployment Stamps | [deployment-stamps/AGENTS.md](deployment-stamps/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/deployment-stamp) |
| Event Sourcing | [event-sourcing/AGENTS.md](event-sourcing/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing) |
| External Configuration Store | [external-configuration-store/AGENTS.md](external-configuration-store/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/external-configuration-store) |
| Federated Identity | [federated-identity/AGENTS.md](federated-identity/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/federated-identity) |
| Gateway Aggregation | [gateway-aggregation/AGENTS.md](gateway-aggregation/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation) |
| Gateway Offloading | [gateway-offloading/AGENTS.md](gateway-offloading/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading) |
| Gateway Routing | [gateway-routing/AGENTS.md](gateway-routing/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing) |
| Geode | [geode/AGENTS.md](geode/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes) |
| Health Endpoint Monitoring | [health-endpoint-monitoring/AGENTS.md](health-endpoint-monitoring/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring) |
| Index Table | [index-table/AGENTS.md](index-table/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/index-table) |
| Leader Election | [leader-election/AGENTS.md](leader-election/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/leader-election) |
| Materialized View | [materialized-view/AGENTS.md](materialized-view/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/materialized-view) |
| Messaging Bridge | [messaging-bridge/AGENTS.md](messaging-bridge/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/messaging-bridge) |
| Pipes and Filters | [pipes-and-filters/AGENTS.md](pipes-and-filters/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters) |
| Priority Queue | [priority-queue/AGENTS.md](priority-queue/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/priority-queue) |
| Publisher-Subscriber | [publisher-subscriber/AGENTS.md](publisher-subscriber/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber) |
| Quarantine | [quarantine/AGENTS.md](quarantine/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/quarantine) |
| Queue-Based Load Leveling | [queue-based-load-leveling/AGENTS.md](queue-based-load-leveling/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling) |
| Rate Limiting | [rate-limiting/AGENTS.md](rate-limiting/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern) |
| Retry | [retry/AGENTS.md](retry/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry) |
| Saga | [saga/AGENTS.md](saga/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/saga) |
| Scheduler Agent Supervisor | [scheduler-agent-supervisor/AGENTS.md](scheduler-agent-supervisor/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/scheduler-agent-supervisor) |
| Sequential Convoy | [sequential-convoy/AGENTS.md](sequential-convoy/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/sequential-convoy) |
| Sharding | [sharding/AGENTS.md](sharding/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding) |
| Sidecar | [sidecar/AGENTS.md](sidecar/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar) |
| Static Content Hosting | [static-content-hosting/AGENTS.md](static-content-hosting/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/static-content-hosting) |
| Strangler Fig | [strangler-fig/AGENTS.md](strangler-fig/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig) |
| Throttling | [throttling/AGENTS.md](throttling/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/throttling) |
| Valet Key | [valet-key/AGENTS.md](valet-key/AGENTS.md) | [Docs](https://learn.microsoft.com/en-us/azure/architecture/patterns/valet-key) |

## Maintaining The Catalog

When Microsoft adds, renames, or removes a cloud design pattern:

1. Add or update the matching pattern folder.
2. Keep the folder slug stable and readable.
3. Update the source link inside the folder's `AGENTS.md`.
4. Update the table in this README.
5. Keep each `AGENTS.md` focused on agent behavior: scope, required shape,
   prohibited drift, and review checks.

## License

This repository is licensed under the MIT License. See [LICENSE](LICENSE).
