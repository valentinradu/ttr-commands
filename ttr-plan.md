# ttr-plan

Requirements → technical → tests.

## State

- No files → Requirements
- `{feature}-req.md` → Technical
- `{feature}-tech.md` → Tests
- All docs → Done (use `/ttr-stub`)

Before advancing, check doc completeness. `AskUserQuestion: "Doc incomplete. Continue or refine?"`

## Config

First use: Ask docs location (default `/docs`). Save to `.ttr.toml`:
```toml
docsPath = "/docs"
```
Docs at `{docsPath}/{feature}/`.

## Requirements (`{feature}-req.md`, 600-1000 tokens)

- Purpose: Problem solved
- Behavior: Inputs, outputs, edge cases
- Constraints: Excluded scope
- Acceptance: Verification criteria

No technical solutions. Ask if ambiguous.
`AskUserQuestion: "Requirements complete? (yes/refine/abandon)"`

## Technical (`{feature}-tech.md`, 400-800 tokens)

Read requirements first.

- Approach: Component breakdown
- Dependencies: Libraries (justify, check licenses, maintenance)
- Interfaces: Signatures, types, contracts only
- Failure modes: How components fail, error handling
- Risks: Problems, mitigation

No implementation. Minimal specs only.
`AskUserQuestion: "Approach sound? (yes/revise/back)"`

## Tests (`{feature}-tests.md`, 300-600 tokens/component)

Read requirements + technical first.

- Components: What's tested
- Behaviors: Expected functionality
- Edge cases: Boundaries, empty/max, invalid, concurrency
- Failures: For each failure mode, expected behavior
- Coverage: Critical paths vs utilities

`AskUserQuestion: "Test plan complete? (yes/add/back)"`

## Usage

```bash
/ttr-plan feature-name
```
