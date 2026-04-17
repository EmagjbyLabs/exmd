# AST

## Purpose

This document defines the canonical exmd v0.1 AST shape.

## General Rules

- Every AST node has a required `type` field.
- Every AST node has a required `span` field.
- Field names are stable.
- Unknown node types are invalid in v0.1.

Type sketches in this document use Rust-oriented pseudotypes.

## Document Node

```text
Document {
  type: "Document",
  metadata: Option<Map<String, JsonValue>>,
  children: Block[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `metadata`
- `children`
- `span`

## Block Nodes

### Paragraph

```text
Paragraph {
  type: "Paragraph",
  children: Inline[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `children`
- `span`

### Heading

```text
Heading {
  type: "Heading",
  level: u8,
  children: Inline[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `level`
- `children`
- `span`

### List

```text
List {
  type: "List",
  ordered: bool,
  items: ListItem[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `ordered`
- `items`
- `span`

### ListItem

```text
ListItem {
  type: "ListItem",
  children: Block[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `children`
- `span`

### Blockquote

```text
Blockquote {
  type: "Blockquote",
  children: Block[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `children`
- `span`

### CodeBlock

```text
CodeBlock {
  type: "CodeBlock",
  fence: String,
  info: Option<String>,
  value: String,
  span: SourceSpan
}
```

Required fields:

- `type`
- `fence`
- `info`
- `value`
- `span`

### HorizontalRule

```text
HorizontalRule {
  type: "HorizontalRule",
  span: SourceSpan
}
```

Required fields:

- `type`
- `span`

### DirectiveBlock

```text
DirectiveBlock {
  type: "DirectiveBlock",
  name: String,
  attributes: DirectiveAttributes,
  children: Block[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `name`
- `attributes`
- `children`
- `span`

## Inline Nodes

### Text

```text
Text {
  type: "Text",
  value: String,
  span: SourceSpan
}
```

Required fields:

- `type`
- `value`
- `span`

### Emphasis

```text
Emphasis {
  type: "Emphasis",
  children: Inline[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `children`
- `span`

### Strong

```text
Strong {
  type: "Strong",
  children: Inline[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `children`
- `span`

### CodeSpan

```text
CodeSpan {
  type: "CodeSpan",
  value: String,
  span: SourceSpan
}
```

Required fields:

- `type`
- `value`
- `span`

### Link

```text
Link {
  type: "Link",
  href: String,
  children: Inline[],
  span: SourceSpan
}
```

Required fields:

- `type`
- `href`
- `children`
- `span`

## Directive Attributes

```text
DirectiveAttributes = {
  BTreeMap<String, String>
}
```

Rules:

- Keys are unique within one directive node.
- Values are strings only.
- Duplicate keys are invalid.

## Node Families

```text
Block =
  Paragraph |
  Heading |
  List |
  ListItem |
  Blockquote |
  CodeBlock |
  HorizontalRule |
  DirectiveBlock

Inline =
  Text |
  Emphasis |
  Strong |
  CodeSpan |
  Link
```
