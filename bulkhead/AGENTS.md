# Bulkhead Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Bulkhead pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead

## Pattern Boundary
Use this pattern to isolate parts of a workload into separate resource pools so one failing or saturated part does not take down the rest.

Isolation can apply to threads, connection pools, queues, tenants, service instances, dependencies, partitions, or compute pools.

## Required Shape
- Identify the isolated compartments and the failure each compartment contains.
- Allocate separate capacity limits, queues, pools, or runtime boundaries per compartment.
- Define saturation behavior, backpressure, and rejection behavior for each compartment.
- Add metrics that show per-bulkhead utilization and failures.
- Test that one exhausted bulkhead does not exhaust the others.

## Do Not Build
- Do not use one global resource pool for all traffic or tenants.
- Do not allow fallback paths to bypass isolation and recreate shared failure.
- Do not hide saturation from callers or operators.
- Do not combine unrelated workloads into a single compartment without a capacity reason.

## Review Checklist
- The isolation boundary is explicit in code or infrastructure.
- Failure containment is tested, not just described.
- Operators can see which bulkhead is saturated and why.
