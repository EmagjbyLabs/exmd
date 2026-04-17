# Directive Decision

## Decision

Directive blocks are part of the main v0.1 spec.

## Why

- They are the main extension mechanism.
- They provide structure without raw HTML.
- They map cleanly to AST nodes and renderer hooks.
- They give Labs-specific components a stable syntax surface.

## Contract

- Opening form: `::name{key="value"}`
- Closing form: `::`
- Attribute values are quoted strings only
- Duplicate attributes are invalid
- Unknown directives are invalid unless registered
