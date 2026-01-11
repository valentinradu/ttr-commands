# ttr-implement

TDD: test → implement → verify. Update manifest.

## Prerequisites

Need stubs + tests. `AskUserQuestion: "Stubs/tests missing. Run /ttr-stub or create tests or continue (will fail)?"`

## Rhythm (per function)

1. Verify test exists
2. Run tests (confirm red)
3. Implement minimal to pass
4. Run tests (confirm green)
5. Update manifest: status = "done", locations = implementation
6. Next function

## Context (~1000 tokens)

Include: stub, tests, direct dependencies, types. Exclude unrelated code.

## Prompt template

```
Implement {function} at {file}:{line}
Constraints: {signature}, {tests}, {dependencies}, {spec excerpt}
Make tests green. Nothing more.
Stop if tests unclear or dependencies missing.
```

## Model routing

- High constraint (clear stub/tests, simple): Low-tier/local
- Medium (some ambiguity, complex): Mid-tier
- Low (unclear reqs, missing tests): Frontier

## Test-first

No test? `AskUserQuestion: "No test. Write first (recommended) or skip (breaks workflow)?"`

## Failures

3 attempts fail? `AskUserQuestion: "Cannot pass. Review spec/review tests/get help?"`

## Order

Dependency order: leaves → top-level.
Circular? `AskUserQuestion: "Circular {A}↔{B}. Refactor/interface/together (risky)?"`

## Manifest

Update `{feature}-manifest.toml` as tasks complete:
```toml
[[tasks]]
name = "Implement parse_config"
status = "done"
locations = ["src/config.rs:42", "tests/config_test.rs:15"]
```

## Usage

```bash
/ttr-implement feature-name
/ttr-implement feature-name function-name
```
