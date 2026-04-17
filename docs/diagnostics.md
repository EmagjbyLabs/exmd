# Diagnostics

## Purpose

This document defines the canonical diagnostic shape for exmd v0.1.

## Diagnostic Shape

```text
Diagnostic {
  code: &'static str,
  message: String,
  severity: "error",
  location: SourceSpan
}
```

Required fields:

- `code`
- `message`
- `severity`
- `location`

## Diagnostic Report Shape

```text
DiagnosticReport {
  errors: Vec<Diagnostic>
}
```

v0.1 uses `errors` only.

## Severity

The only severity level in v0.1 is `error`.

Warnings and informational diagnostics are out of scope for v0.1.

## Rules

- Invalid syntax produces at least one diagnostic.
- Unsupported syntax produces at least one diagnostic.
- A failing compile must not emit a partial success result.
- Diagnostic messages must be deterministic for the same invalid input.

## Error Codes

The following codes are locked for the current v0.1 surface:

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

## Code Meanings

### `E_DIRECTIVE_DUPLICATE_ATTRIBUTE`

Raised when the same attribute key appears more than once in one directive opening line.

### `E_UNKNOWN_DIRECTIVE`

Raised when a directive name is not registered by policy.

### `E_FRONTMATTER_INVALID_JSON`

Raised when frontmatter exists but is not valid JSON.

### `E_DIRECTIVE_UNCLOSED`

Raised when a directive block has an opening delimiter and no matching closing `::`.

### `E_LIST_INVALID_INDENT`

Raised when nested list indentation does not use exactly 2 spaces per level.

### `E_UNSUPPORTED_INLINE_SYNTAX`

Raised when inline syntax is structurally recognized but not supported in v0.1.

### `E_UNSUPPORTED_BLOCK_SYNTAX`

Raised when block syntax is structurally recognized but not supported in v0.1.

### `E_RAW_HTML_DISABLED`

Raised when raw HTML appears while raw HTML is disabled by policy.

### `E_LINK_MALFORMED`

Raised when inline link syntax is incomplete or structurally invalid.

### `E_CODE_BLOCK_UNCLOSED`

Raised when a fenced code block has an opening fence and no matching closing fence.
