# ttr-review

AI-assisted code review with pre-review explanation.

## Prerequisites

Check: feature implemented, tests passing, audit complete. If audit missing: `AskUserQuestion: "Audit not run. Run /ttr-audit first/skip/cancel?"`

## Pre-review explanation

Generate summary (300-500 tokens):
- What changed: File-by-file
- Why: Connect to requirements
- Key decisions: Technical choices, alternatives
- Risk areas: Complex logic, security-sensitive, performance-critical, shared interfaces
- Test coverage: What's tested/untested with justification

Save to `{feature}-review.md`

## Change classification

Per file:
- Cosmetic (formatting, naming, comments) → Low scrutiny
- Behavioral (logic changes) → Medium scrutiny
- Architectural (structure, interfaces) → High scrutiny

## Review prompts

Security: "Review auth checks in {file}:{lines}", "Verify input validation", "Check authorization"
Complex: "Verify algorithm correctness", "Check edge cases vs test plan", "Confirm error handling"
Performance: "Review allocations", "Check query efficiency", "Verify caching"

## Catching inconsistencies

AI explains intention → Human verifies code matches → Mismatch = bug

## Review interface

`AskUserQuestion: "Review {file}:{lines}. Context: {explanation}. Assessment: approve/request changes/needs deeper review?"`

## Confidence signals

- High confidence + cosmetic → Auto-approve
- High confidence + behavioral → Quick scan
- Medium/low confidence → Full review
- Architectural → Always full review

## Auto-approve criteria

All must be true: cosmetic only, tests pass, audit clean, no interface changes, high AI confidence

## Review cycle

1. Generate pre-review
2. Present to reviewer
3. Reviewer provides feedback
4. If changes: implement test-first, re-audit, re-review
5. If approved: commit with reviewed-by tag

## Commit format

```
type({feature}): {description}

{Pre-review explanation}

Changes: {list}
Review: {reviewer}
```

## Usage

```bash
/ttr-review feature-name
/ttr-review feature-name --scope file.rs
```
