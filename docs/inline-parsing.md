# Inline Parsing

## Purpose

This document defines how exmd v0.1 extracts inline text segments from block content and which block nodes allow inline parsing.

## Inline-Capable Blocks

Inline parsing is allowed only in these block nodes:

- `Paragraph`
- `Heading`

Inline parsing is not performed directly on these block nodes:

- `Document`
- `List`
- `ListItem`
- `Blockquote`
- `CodeBlock`
- `HorizontalRule`
- `DirectiveBlock`

Container blocks hold child blocks. Inline parsing happens only when one of those child blocks is inline-capable.

## Input to the Inline Parser

The inline parser receives one normalized text segment for each inline-capable block.

### Paragraph Segment

- A paragraph segment is formed from the paragraph's source lines after block parsing.
- Consecutive paragraph lines are joined with a single ASCII space.
- Block markers, list markers, blockquote markers, and directive delimiters are not part of the segment.

Example:

```text
quoted text
still quoted
```

becomes:

```text
quoted text still quoted
```

### Heading Segment

- A heading segment is the text after the heading marker and the required following space.
- The heading marker is not part of the segment.
- Heading level is determined during block parsing, not inline parsing.

## Text Segmentation Rules

The inline parser processes the segment from left to right.

- Text that does not start a supported inline construct becomes a `Text` node.
- Supported inline constructs are emphasis, strong, code span, and inline link.
- Inline parsing is deterministic. The parser does not use browser-style heuristics.

## Supported Inline Constructs

- `*text*` produces `Emphasis`
- `**text**` produces `Strong`
- `` `code` `` produces `CodeSpan`
- `[text](url)` produces `Link`

## Unsupported Inline Constructs

These forms are invalid in v0.1:

- `_text_`
- `__text__`
- reference-style links
- inline HTML

Unsupported inline syntax produces diagnostics.

## Code Spans

- Code span content is taken as raw inline text.
- Nested inline parsing does not occur inside `CodeSpan`.
- The opening and closing backticks are not part of the `value` field.

## Links

- Link text is parsed as inline content.
- Link destination is stored as `href`.
- Malformed link syntax is invalid.

## Escaping

- Backslash escaping applies only to characters with inline syntax meaning.
- Escaped characters become plain text in the resulting segment.
- Unsupported escape targets are treated as plain text.

## Output Contract

The inline parser emits a sequence of inline nodes.

- The sequence preserves source order.
- Adjacent literal content may be merged into a single `Text` node.
- Inline nodes must cover the full normalized input segment with no implicit gaps.
