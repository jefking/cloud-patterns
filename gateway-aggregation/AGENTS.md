# Gateway Aggregation Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Aggregation pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation

## Pattern Boundary
Use this pattern when a client would otherwise need to call multiple backend services and combine the results itself.

The gateway exposes a client-oriented endpoint that calls several backend services, handles partial failures deliberately, and returns an aggregated response.

## Required Shape
- Identify the client request and the backend calls that must be aggregated.
- Keep composition, timeout, retry, and partial-failure policy in the gateway.
- Return a response shaped for the client, not for backend topology.
- Add observability for each downstream call inside the aggregate operation.
- Test success, partial failure, timeout, and degraded response behavior.

## Do Not Build
- Do not make the client coordinate multiple backend calls for the same operation.
- Do not turn the gateway into the source of record for business state.
- Do not use this folder for route-only gateway behavior without aggregation.
- Do not hide backend failures as complete success unless the contract says so.

## Review Checklist
- The gateway reduces client chattiness and coupling.
- Aggregation policy is explicit for each downstream dependency.
- The client contract remains stable when backend topology changes.
