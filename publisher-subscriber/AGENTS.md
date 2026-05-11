# Publisher-Subscriber Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Publisher-Subscriber pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber

## Pattern Boundary
Use this pattern when publishers should announce events asynchronously to multiple independent subscribers without knowing who receives them.

Publishers emit events to a broker, topic, or event bus. Subscribers independently receive and handle the events they are interested in.

## Required Shape
- Identify publishers, event channels, event contracts, and subscribers.
- Keep publishers decoupled from subscriber implementations and counts.
- Make events immutable facts or notifications with versioned schemas.
- Handle subscriber retries, dead-lettering, duplicate delivery, and idempotency.
- Provide correlation and observability across publish and subscribe flows.

## Do Not Build
- Do not call each subscriber directly from the publisher.
- Do not use pub-sub for a command that must be handled by exactly one receiver.
- Do not let one subscriber failure block delivery to all other subscribers.
- Do not change event schemas without compatibility handling.

## Review Checklist
- New subscribers can be added without publisher changes.
- Each subscriber owns its own processing and failure behavior.
- Event contracts are stable, versioned, and observable.
