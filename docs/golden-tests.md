# Golden Tests

## Purpose

Golden files freeze expected behavior for the parser, renderer, and full compile pipeline.

If a golden file changes, either the spec changed or the implementation regressed. The change should be reviewed as a behavior change, not as incidental test output.

## Layout

```text
tests/golden/
  valid/<case>/
    input.exmd
    ast.json
    html.out
  invalid/<case>/
    input.exmd
    diagnostics.json
```

## Layers

- Parser goldens: `input.exmd -> ast.json` or `diagnostics.json`
- Renderer goldens: `ast.json -> html.out`
- End-to-end goldens: `input.exmd -> html.out`

## Guidance

- Keep fixture names stable and descriptive.
- Add a new fixture when the spec grows or an edge case is clarified.
- Do not rewrite fixtures casually.
- Keep source spans out of early AST goldens until span semantics are stable.
