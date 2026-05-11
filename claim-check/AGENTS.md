# Claim Check Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Claim Check pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/claim-check

## Pattern Boundary
Use this pattern when a message is too large or too sensitive to send directly through a messaging system. Store the payload elsewhere and send a lightweight claim check reference through the bus.

The message carries the reference. The payload store holds the data. Consumers redeem the reference under controlled access.

## Required Shape
- Separate message metadata from the large or protected payload.
- Store payloads in a durable location with scoped access controls.
- Put only the claim check reference and required metadata on the message bus.
- Define payload lifetime, cleanup, integrity checks, and access auditing.
- Handle missing, expired, or unauthorized claim checks explicitly.

## Do Not Build
- Do not place the full large payload on the message bus.
- Do not issue unrestricted or long-lived payload references.
- Do not leave orphaned payloads after message failure or expiration.
- Do not let consumers infer payload location through undocumented conventions.

## Review Checklist
- The message remains small and safe for the transport.
- Payload access is least-privilege and time-bounded.
- Cleanup and failure paths prevent orphaned or leaked payloads.
