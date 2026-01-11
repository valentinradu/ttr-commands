# ttr-implement

TDD: test → implement → verify.

## Prerequisites

Need stubs + tests. `AskUserQuestion: "Stubs/tests missing. Run /ttr-stub or create tests or continue (will fail)?"`

## Rhythm (per function)

1. Verify test exists
2. Run tests (confirm red)
3. Implement minimal to pass
4. Run tests (confirm green)
5. Next function

## Context (~1000 tokens)

Include: stub, tests, direct dependencies, types. Exclude unrelated code.

## Prompt template

```
Implement {function} at {file}:{line}
Constraints: {signature}, {tests}, {dependencies}, {tech excerpt}
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

3 attempts fail? `AskUserQuestion: "Cannot pass. Review tech plan/review tests/get help?"`

## Order

Dependency order: leaves → top-level.
Circular? `AskUserQuestion: "Circular {A}↔{B}. Refactor/interface/together (risky)?"`

## Manifest

Update `{feature}-impl-manifest.json`:
```json
{
  "feature": "feature-name",
  "files": ["src/feature.rs", "tests/feature_test.rs"],
  "functions": ["fn_1", "fn_2"],
  "timestamp": "ISO8601"
}
```

## Usage

```bash
/ttr-implement feature-name
/ttr-implement feature-name function-name
```
