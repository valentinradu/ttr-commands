# ttr-consolidate

Create permanent docs, clean up.

## When

After complete, tested, reviewed.
Incomplete? `AskUserQuestion: "Incomplete ({status}). Consolidate anyway?"`

## Inputs

Read: `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`, `{feature}-review.md`, `{feature}-audit.md`, code, tests

## Output (`{feature}.md`, 400-800 tokens)

```markdown
# {Feature}

## Purpose
Problem solved (from req)

## Architecture
How it works, components, flow (from tech + diagram)

## Key decisions
Libraries, alternatives, trade-offs (from tech + review)

## Usage
How to use, examples

## Testing
Coverage, where tests are (from plan + tests)

## Maintenance
Limitations, future work, attention areas (from audit + review)
```

## Prompt

```
Read {feature} artifacts: {paths}
Create {docs-path}/{feature}.md
Focus: decisions, architecture, trade-offs, locations
Omit: redundant, temporary, resolved, process
```

## Validation

`AskUserQuestion: "Review [preview]. Approve (deletes planning)/revise/cancel?"`

## Cleanup

After approval: `AskUserQuestion: "Delete planning ({list})? In git history, key info in doc. Delete/archive/keep?"`

## ADR

Architectural? `AskUserQuestion: "Create ADR for {decisions}? /docs/adr/in doc/no?"`

```markdown
# ADR-{n}: {title}
Date, Status
## Context: why needed
## Decision: what decided
## Consequences: pros/cons
## Alternatives: rejected options
```

## Usage

```bash
/ttr-consolidate feature-name
/ttr-consolidate feature-name --keep-planning
```
