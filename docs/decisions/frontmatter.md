# Frontmatter Decision

## Decision

exmd v0.1 uses JSON frontmatter only.

## Why

- It keeps the parser boundary simple.
- It avoids YAML-specific ambiguity and feature creep.
- It is easy to validate and easy to cache.
- It matches the goal of deterministic parsing.

## Consequence

Documents with YAML frontmatter are invalid in v0.1.
