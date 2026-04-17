# exmd Spec

## Purpose

exmd is a strict, extensible Markdown dialect for server-rendered content systems. It is designed for deterministic parsing, stable HTML output, safe storage, and controlled extension through directives.

## v0.1 Scope

Supported:

- JSON frontmatter at document start
- Paragraphs
- ATX headings (`#` through `######`)
- Unordered lists (`- item`)
- Ordered lists (`1. item`)
- Blockquotes (`> quote`)
- Fenced code blocks only
- Horizontal rules (`---`)
- Emphasis (`*text*`)
- Strong (`**text**`)
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

## Parsing Model

Parsing is block-first.

1. Normalize source text
2. Parse optional JSON frontmatter
3. Parse blocks in one pass where possible
4. Parse inlines only inside inline-capable blocks
5. Validate directives and attributes
6. Emit a canonical AST
7. Render HTML or JSON AST

## Core Rules

### Frontmatter

- Allowed only at the start of the document
- Delimited by `---` lines
- Content must be valid JSON

### Headings

- Only ATX headings are allowed
- Heading markers must be `#` to `######`
- A space after the marker is required

### Lists

- Unordered list marker is `-`
- Ordered list marker is `<digits>.`
- Marker must be followed by a space
- Nested lists must use exactly 2 spaces per level

### Blockquotes

- Each blockquote line starts with `>`
- One optional space may follow `>`

### Code Blocks

- Only fenced code blocks are supported
- Fence length is at least 3 backticks
- Info string is optional
- Contents are raw text and do not receive inline parsing

### Horizontal Rules

- The only canonical form is `---`

### Inline Syntax

- `*text*` for emphasis
- `**text**` for strong text
- `` `code` `` for inline code
- `[text](url)` for inline links

## Directives

Directive blocks are part of the core v0.1 contract.

Opening form:

```text
::name{key="value"}
```

Closing form:

```text
::
```

Rules:

- Attribute values are quoted strings only
- Duplicate attributes are invalid
- Unknown directives are invalid unless registered by policy
- Directive contents are parsed as block content

## Validation

exmd v0.1 is strict.

- Invalid syntax produces diagnostics
- Unsupported syntax produces diagnostics
- The parser does not silently reinterpret malformed input into another construct

## Rendering

- HTML output is deterministic
- Text is escaped by default
- Raw HTML passthrough is disabled by default
- Directives render only when registered and valid

## AST Contract

The AST is the canonical internal representation.

Core document shape:

```text
Document {
  metadata?,
  children[],
  spans
}
```

Core block nodes:

- `Paragraph`
- `Heading`
- `List`
- `ListItem`
- `Blockquote`
- `CodeBlock`
- `HorizontalRule`
- `DirectiveBlock`

Core inline nodes:

- `Text`
- `Emphasis`
- `Strong`
- `CodeSpan`
- `Link`

## Cache Contract

Compiled artifacts should be keyed by:

```text
content_hash + parser_version + renderer_version + policy_version
```
