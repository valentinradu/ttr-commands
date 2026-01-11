# ttr:audit

Context-aware quality checks. Free-form parameters.

## Parameters

Parse natural language: `/ttr:audit security`, `/ttr:audit performance on http handlers`, `/ttr:audit error handling in auth`

Extract: concern, scope, depth

## Context detection

- Planning docs only → Audit plans
- Implementation exists → Audit code
- Both → Audit consistency

## Audit plans

Requirements: Missing edge cases? Ambiguous criteria? Contradictions? Unstated assumptions?
Technical: Dependencies justified? Failure modes covered? Interfaces clear? Risks identified?
Tests: Coverage gaps? Edge cases from requirements covered? Failure modes tested?

Output: Findings with severity (critical/major/minor)

## Audit code

Security: Input validation, SQL injection, XSS, auth/authz, secrets, dependency CVEs
Performance: Unnecessary allocations, N+1 queries, missing indexes, inefficient algorithms, lock contention
Correctness: Off-by-one, null handling, integer overflow, concurrent access bugs, resource leaks
Maintainability: Complex functions, missing error context, magic numbers, inconsistent naming

Output: Findings ranked by severity and fix effort

## Audit consistency

Compare plans vs implementation:
- Code implements all requirements?
- Code follows technical plan?
- All planned tests implemented?
- Features not in requirements?
- Tests not in test plan?

Output: Discrepancies requiring reconciliation

## Test-first fix (mandatory)

For every finding:
1. Write test reproducing issue (must fail)
2. Verify test fails
3. Implement fix
4. Verify test passes
5. Commit test + fix together

Cannot write failing test? `AskUserQuestion: "Cannot reproduce {finding}. False positive/not understood/test infrastructure missing. Investigate/skip/get review?"`

## Prioritization

| Severity | Effort | Priority |
|----------|--------|----------|
| Critical | Any | P0 |
| Major | Low | P1 |
| Major | High | P2 |
| Minor | Low | P3 |
| Minor | High | P4 |

## Audit chaining

After fixes: `AskUserQuestion: "Fixes complete. Re-audit/move to review/continue implementation?"`

## Usage

```bash
/ttr:audit security
/ttr:audit performance in api handlers
/ttr:audit consistency
/ttr:audit
```
