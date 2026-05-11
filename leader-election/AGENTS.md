# Leader Election Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Leader Election pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/leader-election

## Pattern Boundary
Use this pattern when a distributed set of instances needs exactly one active coordinator or task owner at a time.

Instances compete for leadership through a lease, lock, consensus primitive, or platform-provided election mechanism.

## Required Shape
- Identify the singleton responsibility that only the leader performs.
- Use a lease, lock, epoch, or fencing token with expiration and renewal.
- Ensure followers can detect leadership changes and avoid performing leader-only work.
- Handle leader crash, slow leader, network partition, and clock drift risks.
- Test failover and split-brain prevention.

## Do Not Build
- Do not hardcode a permanent leader instance.
- Do not rely on in-memory flags without a distributed coordination mechanism.
- Do not allow an old leader to keep writing after its lease expires.
- Do not make all instances perform the leader-only task.

## Review Checklist
- Only the current leader can perform protected work.
- Leadership can move after failure without manual intervention.
- Fencing or equivalent protection prevents stale leaders from corrupting state.
