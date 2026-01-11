# ttr-stub

Generate interface stubs from plans.

## Prerequisites

Need: `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`
Missing? `AskUserQuestion: "Planning incomplete. Run /ttr-plan or continue (risky)?"`

## Structure

```
/// Description from tech plan
/// Parameters: ranges/constraints
/// Returns: success/error cases
/// Errors: from failure modes
function component(params) -> Result {
    assert(preconditions_from_tests)
    // TODO: Implement from tests
}
```

## Process

Read all plans. Per component:
1. Extract interface from tech
2. Add docs from requirements
3. Add assertions from test edges
4. Generate stub + TODO

Organize: public first, private after, group related.

Validate: Compile/type check. `AskUserQuestion: "Stubs correct? (yes/fix)"`

If types unclear: `AskUserQuestion: "Types missing. Infer/back to plan/provide?"`
Multiple modules: `AskUserQuestion: "All modules or one at a time?"`

## Manifest

Save `{feature}-stub-manifest.json`:
```json
{
  "feature": "feature-name",
  "files": ["src/feature.rs"],
  "timestamp": "ISO8601"
}
```

## Usage

```bash
/ttr-stub feature-name
```
