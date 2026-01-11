# ttr-review

AI-assisted code review. Split trivial/non-trivial.

## Prerequisites

Need: feature implemented, tests passing.
Missing? `AskUserQuestion: "Tests not passing. Run tests/skip/cancel?"`

## Classification

Split all changes into:

**Trivial:**
- Format (whitespace, line breaks)
- Naming (renames that don't change behavior)
- Comments (additions, fixes, typos)
- Imports (reordering, cleanup)
- Type annotations (adding/fixing without logic change)

**Non-trivial:**
- Logic (any behavioral change)
- Architecture (structure, interfaces)
- Security (auth, validation, secrets)
- Performance (algorithms, queries, allocations)
- Error handling (new cases, changed behavior)

## Trivial Commit

After classification:
`AskUserQuestion: "Found {n} trivial changes ({list}). Commit now? (yes/review first/skip)"`

If yes: Create commit with message "chore: trivial fixes ({summary})"

## Pre-review (300-500 tokens)

Generate `{feature}-review.md` for non-trivial changes:
- Changes: File-by-file
- Why: Link to requirements
- Decisions: Choices, alternatives
- Risks: Complex, security, performance, interfaces
- Coverage: Tested/untested + why

## Review Prompts

Security: "Review auth {file}:{lines}", "Verify validation", "Check authz"
Complex: "Verify algorithm", "Check edges vs tests", "Error handling"
Performance: "Review allocations", "Query efficiency", "Caching"

## Inconsistencies

AI explains → Human verifies → Mismatch = bug

## Interface

`AskUserQuestion: "Review {file}:{lines}. Context: {explanation}. Approve/changes/deeper?"`

## Confidence

- High + trivial → Auto (or quick commit)
- High + behavioral → Quick review
- Medium/low → Full review
- Architectural → Always full

## Cycle

1. Classify changes (trivial/non-trivial)
2. Offer trivial commit
3. Generate pre-review for non-trivial
4. Present
5. Feedback
6. Changes? Test-first, re-review
7. Approved? Consolidate

## Usage

```bash
/ttr-review feature-name
/ttr-review feature-name --scope file.rs
/ttr-review feature-name --skip-trivial
```
