# exmd Syntax

## Heading and Paragraph

```exmd
# Hello

This is a paragraph.
```

## Inline Formatting

```exmd
This has *italic*, **bold**, `code`, and [a link](https://example.com).
```

## Lists

```exmd
- one
- two

1. first
2. second
  - nested
```

Nested lists use exactly 2 spaces.

## Blockquote

```exmd
> quoted text
> still quoted
```

## Fenced Code Block

````exmd
```rust
fn main() {
  println!("hi");
}
````

````

## Horizontal Rule

```exmd
Before

---

After
````

## JSON Frontmatter

```exmd
---
{
  "title": "Page",
  "slug": "page"
}
---

# Content
```

## Directive Block

```exmd
::note{kind="info"}
Hello from note.
::
```

## Nested Directives

```exmd
::section{class="hero"}
# Welcome

::note{kind="info"}
Nested content.
::
::
```

## Invalid Examples

These are invalid in v0.1:

```exmd
This is _not supported_.
```

```exmd
<div>Hello</div>
```

```exmd
    indented code
```
