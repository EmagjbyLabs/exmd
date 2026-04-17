# Source Spans

## Purpose

This document defines the source span model for exmd v0.1 AST nodes and diagnostics.

## Span Shape

```text
SourceSpan {
  start: SourcePosition,
  end: SourcePosition
}

SourcePosition {
  offset: usize,
  line: u32,
  column: u32
}
```

## Coordinate Rules

- `offset` is zero-based.
- `line` is one-based.
- `column` is one-based.
- `start` is inclusive.
- `end` is exclusive.

## Source Unit

- Coordinates are measured against the normalized source text.
- Line endings must be normalized before spans are computed.
- Offsets count UTF-8 bytes in the normalized source text.

## Rust Notes

- Byte offsets are the canonical offset unit for v0.1 implementations.
- Byte offsets map directly to Rust string slicing boundaries when the parser tracks valid UTF-8 boundaries.
- `line` and `column` remain human-facing coordinates.

## Node Coverage

- Every AST node has one `span`.
- A node span covers the full source range of the construct.
- Delimiters are included in the node span.

Examples:

- A `Heading` span includes the `#` markers and the following heading text.
- A `CodeBlock` span includes the opening fence, body, and closing fence.
- A `DirectiveBlock` span includes the opening directive line, child content, and closing `::`.

## Parent and Child Spans

- Child spans must be contained within the parent span.
- Sibling spans must not overlap.
- Container node spans may include separators and delimiters not covered by children.

## Metadata Span

- The `Document` span covers the full normalized source.
- Frontmatter, when present, is covered by the `Document` span.
- Metadata values may be stored separately from block nodes. v0.1 does not require per-field metadata spans.

## Diagnostic Locations

- Diagnostics use the same coordinate format as AST spans.
- A diagnostic location should identify the smallest relevant invalid region when possible.
- If no narrow region is available, the diagnostic location may use the enclosing construct span.
