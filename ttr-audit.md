# ttr-audit

Context-aware quality checks. Update manifest.

## Parameters

Parse: `/ttr-audit security`, `/ttr-audit performance on http handlers`, `/ttr-audit error handling in auth`
Extract: concern, scope, depth

## Context

- Docs only → Audit plans
- Code exists → Audit code
- Both → Audit consistency

## Plans

Spec: Edge cases? Ambiguous? Contradictions? Assumptions? Dependencies justified? Failures covered? Interfaces clear? Risks?
Tests: Coverage gaps? Edge cases? Failures tested?

Output: Severity (critical/major/minor)

## Code

Security: Validation, injection, XSS, auth, secrets, CVEs
Performance: Allocations, N+1, indexes, algorithms, locks
Correctness: Off-by-one, nulls, overflow, concurrency, leaks
Maintainability: Complexity, error context, magic numbers, naming

Output: Ranked by severity + effort

## Consistency

Plans vs code:
- All requirements implemented?
- Follows spec?
- All tests implemented?
- Extra features?
- Extra tests?

Output: Discrepancies

## Test-first (mandatory)

Per finding:
1. Write failing test
2. Verify fails
3. Fix
4. Verify passes

Can't reproduce? `AskUserQuestion: "Cannot reproduce {finding}. False positive/unclear/missing infrastructure. Investigate/skip/review?"`

## Priority

| Severity | Effort | Priority |
|----------|--------|----------|
| Critical | Any | P0 |
| Major | Low | P1 |
| Major | High | P2 |
| Minor | Low | P3 |
| Minor | High | P4 |

## Manifest

Add audit findings as tasks in `{feature}-manifest.toml`:
```toml
[[tasks]]
name = "Fix SQL injection in user query"
status = "todo"
locations = ["src/db.rs:87"]
```

## Next

`AskUserQuestion: "Fixes done. Re-audit/review/continue?"`

## Usage

```bash
/ttr-audit security
/ttr-audit performance in api handlers
/ttr-audit consistency
/ttr-audit
```
