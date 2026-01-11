# ttr-plan

Progressive requirements → technical → tests workflow.

## State detection

- No files → Requirements
- `{feature}-req.md` exists → Technical
- `{feature}-tech.md` exists → Tests
- All docs exist → Complete (use `/stub`)

## File completeness

Before advancing, check current doc for missing sections/TODOs/unclear parts. If incomplete:

```
AskUserQuestion: "Document appears incomplete. Continue to next phase or refine first?"
```

## Docs location

First use: Ask where to store docs (default `/docs`). Save to `.psiar-config.json`:

```json
{"docsPath": "/docs"}
```

Feature docs created at `{docsPath}/{feature}/`.

## Requirements

Write `{feature}-req.md` (600-1000 tokens):
- Purpose: What problem this solves
- Behavior: Inputs, outputs, edge cases
- Constraints: What's excluded
- Acceptance: Verification criteria

No technical solutions. Ask clarifying questions if ambiguous.

Validate: `AskUserQuestion: "Requirements complete? (yes/refine/abandon)"`

## Technical

Read requirements. Write `{feature}-tech.md` (400-800 tokens):
- Approach: Component breakdown
- Dependencies: Libraries with justification, licenses, maintenance
- Interfaces: Function signatures, types, contracts (signatures only, no implementation)
- Failure modes: How components fail, error handling
- Risks: What could go wrong, mitigation

No code implementation. Minimal interface specs only if essential for clarity.

Validate: `AskUserQuestion: "Approach sound? (yes/revise/back to requirements)"`

## Tests

Read requirements + technical. Write `{feature}-tests.md` (300-600 tokens/component):
- Components: What's being tested
- Behaviors: Expected functionality
- Edge cases: Boundary values, empty/max inputs, invalid inputs, concurrency
- Failure scenarios: For each failure mode, expected behavior
- Coverage goals: Critical paths vs utilities

Validate: `AskUserQuestion: "Test plan complete? (yes/add more/back)"`

## Usage

```bash
/ttr-plan feature-name
```
