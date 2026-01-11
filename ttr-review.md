# ttr-review

AI-assisted code review.

## Prerequisites

Need: feature implemented, tests passing, audit done.
Missing audit? `AskUserQuestion: "No audit. Run /ttr-audit/skip/cancel?"`

## Pre-review (300-500 tokens)

Generate `{feature}-review.md`:
- Changes: File-by-file
- Why: Link to requirements
- Decisions: Choices, alternatives
- Risks: Complex, security, performance, interfaces
- Coverage: Tested/untested + why

## Classification

Per file:
- Cosmetic (format, naming, comments) → Low
- Behavioral (logic) → Medium
- Architectural (structure, interfaces) → High

## Prompts

Security: "Review auth {file}:{lines}", "Verify validation", "Check authz"
Complex: "Verify algorithm", "Check edges vs tests", "Error handling"
Performance: "Review allocations", "Query efficiency", "Caching"

## Inconsistencies

AI explains → Human verifies → Mismatch = bug

## Interface

`AskUserQuestion: "Review {file}:{lines}. Context: {explanation}. Approve/changes/deeper?"`

## Confidence

- High + cosmetic → Auto
- High + behavioral → Quick
- Medium/low → Full
- Architectural → Always full

## Auto-approve

All true: cosmetic, tests pass, audit clean, no interface changes, high confidence

## Cycle

1. Generate pre-review
2. Present
3. Feedback
4. Changes? Test-first, re-audit, re-review
5. Approved? Consolidate

## Usage

```bash
/ttr-review feature-name
/ttr-review feature-name --scope file.rs
```
