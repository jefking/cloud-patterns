# Messaging Bridge Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Messaging Bridge pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/messaging-bridge

## Pattern Boundary
Use this pattern when two or more messaging systems need to exchange messages but use incompatible protocols, formats, routing models, or delivery semantics.

The bridge is an integration component. It connects messaging systems while preserving intent, metadata, and reliability as far as the systems allow.

## Required Shape
- Identify the source and target messaging systems.
- Translate message format, headers, routing keys, and acknowledgement semantics explicitly.
- Define retry, dead-letter, deduplication, and ordering behavior across the bridge.
- Preserve correlation and trace context where possible.
- Test delivery, duplicate, failure, and poison-message cases.

## Do Not Build
- Do not put domain business processing inside the bridge.
- Do not assume the target system has the same delivery guarantees as the source.
- Do not silently drop headers or metadata required by consumers.
- Do not create a bridge loop without loop detection or routing controls.

## Review Checklist
- Message movement across systems is reliable and observable.
- Semantic differences between brokers are documented in code or tests.
- The bridge can recover from failure without message loss or uncontrolled duplication.
