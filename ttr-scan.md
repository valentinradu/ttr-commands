# ttr-scan

Scan codebase, report state, sync manifest.

## Detection

1. Load `.ttr.toml` (docsPath)
2. Find manifest: `{feature}-manifest.toml`
3. Find docs: `{feature}-spec.md`, `{feature}-tests.md`, `{feature}-review.md`
4. Scan code for TODOs

## TODO Patterns

Language-aware scanning:

| Language | Pattern |
|----------|---------|
| Rust, Go, C, JS, TS, Swift, Java, Kotlin | `// TODO` |
| Python, Ruby, Shell, YAML | `# TODO` |
| Haskell, Lua, SQL | `-- TODO` |
| HTML, XML | `<!-- TODO` |
| CSS | `/* TODO` |
| Rust (lowercase convention) | `// todo` |

Extract: file:line, description, surrounding context.

## Sync

Compare manifest tasks vs code TODOs:
- TODO in code, not in manifest → Add task (status = "todo")
- Task in manifest, TODO removed → Update status = "done"
- Mismatch locations → Update locations

## Report

Output to user:
```
Feature: {name}
Phase: {planning|stubbing|implementing|auditing|reviewing|done}

Tasks:
  [done] Implement parse_config (src/config.rs:42)
  [todo] Implement validate_input (src/input.rs:87)
  [todo] Fix buffer overflow (src/parser.rs:156)

TODOs found: 5
Tasks in manifest: 3
New TODOs (not in manifest): 2
```

## Phase Detection

- No config → `/ttr-plan`
- Spec only → Continue tests
- Docs complete, no stubs → `/ttr-stub`
- Stubs done, tasks pending → `/ttr-implement`
- Tasks done, no review → `/ttr-review`
- Review done → `/ttr-consolidate`

## Context

Summarize (100-200 tokens): requirements, approach, test strategy, pending tasks, current state.

`AskUserQuestion: "Detected {phase}. {summary}. Continue/override/specify?"`

## Usage

```bash
/ttr-scan
/ttr-scan feature-name
```
