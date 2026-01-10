# consolidate

Create permanent docs from planning artifacts. Clean up.

## When

After feature complete, tested, reviewed, merged. If incomplete: `AskUserQuestion: "Feature incomplete ({status}). Consolidate anyway?"`

## Inputs

Read: `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`, `{feature}-review.md`, `{feature}-audit.md`, implemented files, tests, related commits

## Output structure

Create `{feature}.md` (400-800 tokens):

```markdown
# {Feature}

## Purpose
What problem this solves (from requirements)

## Architecture
How it works, key components, data flow (from technical + diagram if helpful)

## Key decisions
Library choices, approach alternatives, trade-offs (from tech plan + review + commits)

## Usage
How to use, API examples

## Testing
Coverage strategy, where tests are (from test plan + actual tests)

## Maintenance
Known limitations, future work, attention areas (from audit + review)
```

## Generation prompt

```
Read {feature} artifacts: {paths}
Create consolidated doc at {docs-path}/{feature}.md
Focus: decisions future devs need, non-obvious architecture, trade-offs, where to find things
Omit: redundant info in code, temporary details, resolved issues, process artifacts
```

## Validation

`AskUserQuestion: "Review consolidated doc [preview]. Approve (will delete planning)/revise/cancel?"`

## Cleanup

After approval: `AskUserQuestion: "Delete planning artifacts ({list})? Preserved in git history, key info in consolidated doc. Delete all/archive to /docs/archive/keep (not recommended)?"`

Require explicit confirmation.

## ADR

If architectural decisions: `AskUserQuestion: "Create ADR for {decisions}? Yes in /docs/adr//no, in feature doc/yes, separate?"`

ADR template:
```markdown
# ADR-{n}: {title}
Date, Status, Context
## Context: why decision needed
## Decision: what was decided
## Consequences: positive and negative
## Alternatives: other options, why rejected
```

## Commit

```
docs({feature}): consolidate feature documentation

Created {path}/{feature}.md from planning artifacts.
Deleted planning docs (in git history): {list}
Feature implemented in: {commits}
```

## Usage

```bash
/consolidate feature-name
/consolidate feature-name --keep-planning
```
