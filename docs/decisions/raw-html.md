# Raw HTML Decision

## Decision

Raw HTML is disabled by default in exmd v0.1.

## Why

- It reduces XSS risk in stored content.
- It keeps rendering deterministic.
- It prevents format escape hatches from replacing the directive system.
- It makes validation and sanitization policy explicit.

## Consequence

Authors must use exmd directives instead of embedding raw HTML for structure.
