# Asynchronous Request-Reply Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Asynchronous Request-Reply pattern.

Source: https://learn.microsoft.com/en-us/azure/architecture/patterns/asynchronous-request-reply

## Pattern Boundary
Use this pattern when a caller needs a timely acknowledgement for work that must continue asynchronously after the initial request.

The initial endpoint accepts and validates work, returns a status location, and delegates processing to a background component. The caller checks status and later retrieves the result or failure.

## Required Shape
- Provide an acceptor endpoint that validates the request before starting work.
- Return an accepted response with a status resource location and clear polling guidance.
- Separate the long-running worker from the request path.
- Model status states such as pending, running, succeeded, failed, and canceled.
- Persist enough status, result, and error information for reliable polling.
- Make duplicate submissions safe through idempotency keys or equivalent correlation.

## Do Not Build
- Do not block the initial request until long-running work finishes.
- Do not hide asynchronous state behind a synchronous facade unless that facade is the explicit deliverable.
- Do not use this folder for real-time streaming, push callbacks, or generic queue processing unless request-reply polling remains central.
- Do not return success before validation of the initial request.

## Review Checklist
- The client receives a stable status URL or equivalent status reference.
- Background processing can fail without losing the final observable state.
- Polling behavior avoids unnecessary load and exposes terminal outcomes clearly.
