# ttr-fix

Fix all issues unless filtered. Update manifest.

## Parameters

Parse: `/ttr-fix`, `/ttr-fix ignore P2`, `/ttr-fix only security`, `/ttr-fix ignore minor`
Extract: priority filter, concern filter

## Context

- Docs → Fix docs
- Tests → Fix tests
- Code → Fix code

## Prerequisites

Check `{feature}-manifest.toml` for tasks with status = "todo".

None? `AskUserQuestion: "No issues. Run /ttr-audit/specify/abort?"`

## Docs

Missing sections, TODOs, ambiguities, contradictions.
Unclear? `AskUserQuestion: "{issue}. A: {approach} / B: {approach} / Skip?"`

## Tests

Missing edges, incomplete coverage, mismatches, setup.
Unclear? `AskUserQuestion: "{issue}. Mock/add/refactor/skip?"`

## Code

**Test-first mandatory.**

Per issue:
1. Write failing test
2. Verify fails
3. If unclear: `AskUserQuestion: "{issue}. A: {details} / B: {details} / Investigate?"`
4. Fix minimal
5. Verify passes
6. Update manifest: status = "done"

Can't test? `AskUserQuestion: "Can't reproduce {issue}. False positive/investigate/missing infra?"`

## Filters

- Default: ALL
- `ignore P2` → Skip P2
- `ignore minor` → Critical, major
- `only security` → Security only
- `only P0 P1` → P0, P1 only

Process: P0 → P1 → P2 → P3 → P4

## Gates

Ask before fix if:
- Architectural change
- Multiple approaches
- >3 files impacted
- Contradicts spec
- New dependency
- Unclear if problem

**Never guess. Always ask.**

## Manifest

Update `{feature}-manifest.toml` as fixes complete:
```toml
[[tasks]]
name = "Fix SQL injection in user query"
status = "done"
locations = ["src/db.rs:87", "tests/db_test.rs:120"]
```

## After

Code fixes:
1. Run tests
2. Run linter
3. Check new issues

`AskUserQuestion: "Done. Re-audit/review/continue?"`
>10 issues? `AskUserQuestion: "{count} issues. All/category/priority/one?"`

## Usage

```bash
/ttr-fix
/ttr-fix ignore P2
/ttr-fix only security
/ttr-fix ignore minor
```
