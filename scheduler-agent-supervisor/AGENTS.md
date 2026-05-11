# Scheduler Agent Supervisor Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Scheduler Agent Supervisor pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/scheduler-agent-supervisor

## Pattern Boundary
Use this pattern when a distributed workflow needs scheduling, step execution by agents, and supervision for completion, retries, or recovery.

The scheduler plans work. Agents perform actions. The supervisor monitors progress, detects failures, and decides whether to retry, compensate, or escalate.

## Required Shape
- Separate scheduler, agent, and supervisor responsibilities.
- Persist workflow state, step status, ownership, and retry counts.
- Make agent steps idempotent or duplicate-safe.
- Define supervision rules for timeouts, retries, compensation, and abandonment.
- Test agent failure, scheduler restart, supervisor recovery, and partial completion.

## Do Not Build
- Do not collapse the workflow into one untracked synchronous call chain.
- Do not run background work without persisted progress.
- Do not let failed agents leave work permanently orphaned.
- Do not confuse this pattern with generic cron scheduling without supervision.

## Review Checklist
- Work can resume after process or node failure.
- The supervisor can determine the state of every scheduled action.
- Retry and compensation decisions are explicit and observable.
