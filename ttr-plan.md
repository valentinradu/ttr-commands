# ttr-plan

Create spec and test plan. Update manifest.

## State

- No files → Spec
- `{feature}-spec.md` → Tests
- All docs → Done (use `/ttr-stub`)

Before advancing, check doc completeness. `AskUserQuestion: "Doc incomplete. Continue or refine?"`

## Config

First use: Ask docs location (default `/docs`). Save to `.ttr.toml`:
```toml
docsPath = "/docs"
```
Docs at `{docsPath}/{feature}/`.

## Spec (`{feature}-spec.md`, 800-1200 tokens)

Combines requirements and technical design.

**Requirements:**
- Purpose: Problem solved
- Behavior: Inputs, outputs, edge cases
- Constraints: Excluded scope
- Acceptance: Verification criteria

**Design:**
- Approach: Component breakdown
- Dependencies: Libraries (justify, check licenses, maintenance)
- Interfaces: Signatures, types, contracts only
- Failure modes: How components fail, error handling
- Risks: Problems, mitigation

No implementation code. Ask if ambiguous.
`AskUserQuestion: "Spec complete? (yes/refine/abandon)"`

## Tests (`{feature}-tests.md`, 300-600 tokens/component)

Read spec first.

- Components: What's tested
- Behaviors: Expected functionality
- Edge cases: Boundaries, empty/max, invalid, concurrency
- Failures: For each failure mode, expected behavior
- Coverage: Critical paths vs utilities

`AskUserQuestion: "Test plan complete? (yes/add/back)"`

## Manifest

Update `{feature}-manifest.toml` after each phase:
```toml
[[tasks]]
name = "Implement parse_config"
status = "todo"  # todo | in-progress | done
locations = ["src/config.rs:42", "src/lib.rs:10"]
```

## Usage

```bash
/ttr-plan feature-name
```
