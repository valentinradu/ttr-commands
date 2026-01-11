# ttr-fix

Context-aware issue resolution. Addresses ALL findings unless filtered.

## Parameters

Parse natural language: `/ttr-fix`, `/ttr-fix ignore P2`, `/ttr-fix only security`, `/ttr-fix ignore minor`

Extract: priority filter, concern filter

## Context detection

- Planning docs → Fix doc issues
- Tests only → Fix test issues
- Implementation → Fix code issues
- `{feature}-audit.md` exists → Fix audit findings

## Prerequisites

Check for issues:
1. `{feature}-audit.md` → Audit findings
2. Linter/compiler → Tool issues
3. Test failures → Test issues
4. Docs TODOs/incomplete → Doc issues

No issues: `AskUserQuestion: "No issues found. Run /ttr-audit first/specify what to fix/abort?"`

## Fix docs

Missing sections, TODOs, ambiguities, contradictions, incomplete coverage.

Per issue: If unclear/multiple options → `AskUserQuestion: "{issue}. Option A: {approach} / Option B: {approach} / Skip?"`

Commit: `docs({feature}): fix {category} issues`

## Fix tests

Missing edge cases, incomplete coverage, requirement mismatches, setup problems.

Per issue: If unclear → `AskUserQuestion: "Test issue: {description}. Mock/add case/refactor/skip?"`

Commit: `test({feature}): fix {category} issues`

## Fix code

**Test-first mandatory.**

Per issue:
1. Write failing test (if none exists)
2. Verify test fails
3. If unclear/multiple options: `AskUserQuestion: "{issue}. Approach A: {details} / Approach B: {details} / Investigate?"`
4. Implement minimal fix
5. Verify test passes
6. Commit: `fix({feature}): {issue description}`

Cannot write test: `AskUserQuestion: "Cannot reproduce {issue}. False positive/investigate/missing infrastructure?"`

## Audit findings

Read `{feature}-audit.md`. Apply filters:

- Default: Fix ALL (P0-P4)
- `/ttr-fix ignore P2` → Skip P2
- `/ttr-fix ignore minor` → Critical, major only
- `/ttr-fix only security` → Security findings only
- `/ttr-fix only P0 P1` → P0, P1 only

Process by priority: P0 → P1 → P2 → P3 → P4

Per finding: Apply context-specific workflow above

After fixes: Delete or update audit file

## Validation gates

`AskUserQuestion` before fix if:
- Architectural change needed
- Multiple valid approaches
- Broad impact (>3 files)
- Contradicts plan
- New dependency required
- Unclear if actually a problem

**Never guess. Always ask.**

## After fixes

Code fixes:
1. Run full test suite
2. Run linter/compiler
3. Check for new issues

`AskUserQuestion: "Fixes complete. Re-audit/move to review/continue implementation?"`

If >10 issues: `AskUserQuestion: "{count} issues. Fix all/by category/by priority/one at a time?"`

## Commit format

```
fix({feature}): {category or specific issue}

Addresses: {issues fixed}
Changes: {summary}
```

Docs/tests: One commit per category
Code: One commit per issue (test + fix)

## Usage

```bash
/ttr-fix
/ttr-fix ignore P2
/ttr-fix only security
/ttr-fix ignore minor
```
