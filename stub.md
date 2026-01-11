# ttr:stub

Generate interface stubs from planning docs.

## Prerequisites

Check files exist: `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`

If missing: `AskUserQuestion: "Planning incomplete. Run /ttr:plan first or create stubs anyway (risky)?"`

## Stub structure

For each component in technical plan:

```
/// Description from technical plan.
/// Parameters: document with valid ranges/constraints
/// Returns: including success/error cases
/// Errors: enumerate from failure modes
function component(params) -> Result {
    assert(preconditions_from_test_plan)
    // TODO: Implement specific behavior from tests
}
```

## Generation

Read all planning docs. For each component:
1. Extract interface from technical plan
2. Add documentation from requirements
3. Add assertions from test edge cases
4. Generate stub with TODO placeholder

Organize: public interface first, private after, group related functions.

Validate: Run compiler/type checker. If passes, ask: `AskUserQuestion: "Stubs correct? (yes/needs changes)"`

If technical plan is vague on types: `AskUserQuestion: "Types not specified. Infer/go back to planning/provide now?"`

Multiple modules: `AskUserQuestion: "Create stubs for all modules or one at a time?"`

## Manifest

After stub generation, save `{feature}-stub-manifest.json`:

```json
{
  "feature": "feature-name",
  "files": ["src/feature.rs", "src/feature/mod.rs"],
  "timestamp": "ISO8601"
}
```

## Usage

```bash
/ttr:stub feature-name
```
