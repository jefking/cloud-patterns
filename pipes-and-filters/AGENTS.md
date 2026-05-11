# Pipes and Filters Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Pipes and Filters pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters

## Pattern Boundary
Use this pattern when complex processing should be decomposed into a pipeline of independent processing steps.

Filters perform focused transformations or validations. Pipes carry data between filters and allow the pipeline to be composed, tested, and scaled step by step.

## Required Shape
- Identify each filter and the pipe connecting it to the next step.
- Keep filters focused, reusable, and independently testable.
- Define input and output contracts for every filter.
- Handle per-filter failure, retry, and poison-data behavior.
- Make pipeline ordering and composition explicit.

## Do Not Build
- Do not combine all processing into one opaque monolithic step.
- Do not rely on hidden shared mutable state between filters.
- Do not make filters depend on downstream implementation details.
- Do not skip contract tests at filter boundaries.

## Review Checklist
- Each filter has a clear single processing responsibility.
- Pipes and contracts make the pipeline composition visible.
- Failure in one filter can be isolated, retried, or reported.
