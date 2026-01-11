# ttr-implement

TDD cycle: test → implement → verify → commit.

## Prerequisites

Check stubs compile and tests exist. If missing: `AskUserQuestion: "Stubs/tests missing. Run /ttr-stub or create tests first or continue (will fail)?"`

## Rhythm per function

1. Verify test exists
2. Run tests (confirm red)
3. Implement minimal code to pass
4. Run tests (confirm green)
5. Commit: `feat({feature}): implement {function}`
6. Next function

## Context

Provide only: function stub, function tests, direct dependencies, types used. Exclude unrelated code (~1000 tokens total).

## Implementation prompt

```
Implement {function} at {file}:{line}
Constraints: {signature}, {tests}, {dependencies}, {tech plan excerpt}
Make tests green. Nothing more.
Stop if tests unclear or dependencies missing.
```

## Model routing

- High constraint (clear stub, tests, simple logic): Low-tier/local
- Medium constraint (some ambiguity, complex logic): Mid-tier
- Low constraint (unclear requirements, missing tests): Frontier

## Test-first mandatory

No test for function? `AskUserQuestion: "No test. Write test first (recommended) or skip (breaks workflow)?"`

If skipped, mark commit as risky.

## Failures

If tests don't pass after 3 attempts: `AskUserQuestion: "Cannot pass tests. Review technical plan/review tests/get help?"`

## Commit

```
type({feature}): implement {function}

Makes {test} pass. Implements {brief description}
```

## Function order

Implement in dependency order: leaves first, top-level last.

Circular dependency: `AskUserQuestion: "Circular {A}↔{B}. Refactor design/break with interface/implement together (risky)?"`

## Manifest

Update `{feature}-impl-manifest.json` after each function:

```json
{
  "feature": "feature-name",
  "files": ["src/feature.rs", "tests/feature_test.rs"],
  "functions": ["fn_name_1", "fn_name_2"],
  "timestamp": "ISO8601"
}
```

## Usage

```bash
/ttr-implement feature-name
/ttr-implement feature-name function-name
```
