# ttr-stub

Generate interface stubs from spec. Update manifest.

**NO IMPLEMENTATION. Stubs only.**

## Prerequisites

Need: `{feature}-spec.md`, `{feature}-tests.md`
Missing? `AskUserQuestion: "Planning incomplete. Run /ttr-plan or continue (risky)?"`

## Structure

```
/// Description from spec
/// Parameters: ranges/constraints
/// Returns: success/error cases
/// Errors: from failure modes
function component(params) -> Result {
    // TODO: Implement
    todo!() // or unimplemented!(), panic!(), throw, etc.
}
```

## Rules

- **Signatures only** - no logic, no algorithms, no real code
- **Body = TODO + placeholder** - `todo!()`, `unimplemented!()`, `throw new Error()`, `pass`, etc.
- **Docs from spec** - copy constraints, parameters, return types
- **No "partial" implementations** - if it does anything, it's too much

## Process

Read spec and test plan. Per component:
1. Extract interface from spec
2. Add docs from requirements section
3. Generate stub with TODO placeholder
4. **Stop. Do not implement.**

Organize: public first, private after, group related.

Validate: Compile/type check. `AskUserQuestion: "Stubs correct? (yes/fix)"`

If types unclear: `AskUserQuestion: "Types missing. Infer/back to plan/provide?"`
Multiple modules: `AskUserQuestion: "All modules or one at a time?"`

## Manifest

Update `{feature}-manifest.toml` with stub locations:
```toml
[[tasks]]
name = "Implement parse_config"
status = "todo"
locations = ["src/config.rs:42"]
```

## Usage

```bash
/ttr-stub feature-name
```
