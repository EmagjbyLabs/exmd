# exmd v0.1 Reference

## Purpose

This document combines the locked exmd v0.1 rules into one implementation reference.

## Scope

Supported:

- JSON frontmatter at document start
- Paragraphs
- ATX headings
- Unordered lists
- Ordered lists
- Blockquotes
- Fenced code blocks
- Horizontal rules
- Emphasis with `*`
- Strong with `**`
- Inline code
- Inline links
- Directive blocks with quoted-string attributes

Not supported:

- Raw HTML in default mode
- Setext headings
- Indented code blocks
- `_` emphasis
- Tables
- Footnotes
- Reference links
- Images
- YAML frontmatter

## Parse Order

1. Normalize source text
2. Parse optional JSON frontmatter at document start
3. Parse blocks
4. Parse inlines only inside inline-capable blocks
5. Validate directives and attributes
6. Emit canonical AST
7. Render HTML or JSON AST

## Block Rules

### Frontmatter

- Frontmatter is allowed only at the start of the document.
- It is delimited by `---` lines.
- Its body must be valid JSON.

### Paragraph

- A paragraph is one or more consecutive non-empty lines not claimed by another block rule.
- Paragraph lines are normalized into one inline parsing segment.

### Heading

- Only ATX headings are valid.
- Levels `1` through `6` are valid.
- A space after the heading marker is required.

### List

- Unordered marker: `-`
- Ordered marker: `<digits>.`
- A space after the marker is required.
- Nested list indentation is exactly 2 spaces per level.

### Blockquote

- Each blockquote line starts with `>`.
- One optional space may follow `>`.

### CodeBlock

- Only fenced code blocks are valid.
- Fence length is at least 3 backticks.
- Info string is optional.
- Code block content does not receive inline parsing.

### HorizontalRule

- The only canonical horizontal rule form is `---`.

### DirectiveBlock

- Opening form: `::name{key="value"}`
- Closing form: `::`
- Attribute values are quoted strings only.
- Duplicate attributes are invalid.
- Unknown directives are invalid unless registered by policy.
- Directive contents are parsed as block content.

## Inline Rules

### Inline-Capable Blocks

Only these blocks allow inline parsing:

- `Paragraph`
- `Heading`

All other block nodes are non-inline-capable containers or leaf nodes.

### Inline Segment Rules

- Paragraph lines are joined with a single ASCII space.
- Heading content is the text after the marker and required space.
- Block delimiters and markers are excluded from the inline segment.

### Supported Inline Nodes

- `Text`
- `Emphasis`
- `Strong`
- `CodeSpan`
- `Link`

### Supported Inline Syntax

- `*text*`
- `**text**`
- `` `code` ``
- `[text](url)`

### Inline Constraints

- Code span content does not receive nested inline parsing.
- Link text may contain inline nodes.
- Unsupported inline syntax produces diagnostics.

## AST Rules

### General

- Every AST node has `type` and `span`.
- The `Document` node has `metadata`, `children`, and `span`.

### Block Nodes

- `Paragraph { type, children, span }`
- `Heading { type, level: u8, children, span }`
- `List { type, ordered: bool, items, span }`
- `ListItem { type, children, span }`
- `Blockquote { type, children, span }`
- `CodeBlock { type, fence: String, info: Option<String>, value: String, span }`
- `HorizontalRule { type, span }`
- `DirectiveBlock { type, name: String, attributes, children, span }`

### Inline Nodes

- `Text { type, value: String, span }`
- `Emphasis { type, children, span }`
- `Strong { type, children, span }`
- `CodeSpan { type, value: String, span }`
- `Link { type, href: String, children, span }`

### Directive Attributes

- Attributes are represented as `BTreeMap<String, String>`.
- Keys are unique within a directive.
- Values are strings only.

## Span Rules

- `offset` is zero-based.
- `line` is one-based.
- `column` is one-based.
- `start` is inclusive.
- `end` is exclusive.
- Every node span is measured against normalized source text.
- Offsets count UTF-8 bytes.
- Delimiters are included in the node span.

## Diagnostics

### Shape

```text
Diagnostic {
  code: &'static str,
  message: String,
  severity: "error",
  location: SourceSpan
}
```

### Severity

- v0.1 uses `error` only.

### Locked Codes

- `E_DIRECTIVE_DUPLICATE_ATTRIBUTE`
- `E_UNKNOWN_DIRECTIVE`
- `E_FRONTMATTER_INVALID_JSON`
- `E_DIRECTIVE_UNCLOSED`
- `E_LIST_INVALID_INDENT`
- `E_UNSUPPORTED_INLINE_SYNTAX`
- `E_UNSUPPORTED_BLOCK_SYNTAX`
- `E_RAW_HTML_DISABLED`
- `E_LINK_MALFORMED`
- `E_CODE_BLOCK_UNCLOSED`

## Rendering Rules

- HTML output is deterministic.
- Text is escaped by default.
- Raw HTML passthrough is disabled by default.
- Directives render only when registered and valid.

## Cache Key

Compiled artifacts should be keyed by:

```text
content_hash + parser_version + renderer_version + policy_version
```
