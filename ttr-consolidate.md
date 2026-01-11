# ttr-consolidate

Create permanent docs, clean up.

## When

After complete, tested, reviewed.
Incomplete? `AskUserQuestion: "Incomplete ({status}). Consolidate anyway?"`

## Inputs

Read: `{feature}-spec.md`, `{feature}-tests.md`, `{feature}-review.md`, `{feature}-manifest.toml`, code, tests

## Output (`{feature}.md`, 400-800 tokens)

```markdown
# {Feature}

## Purpose
Problem solved (from spec)

## Architecture
How it works, components, flow (from spec + diagram)

## Key decisions
Libraries, alternatives, trade-offs (from spec + review)

## Usage
How to use, examples

## Testing
Coverage, where tests are (from plan + tests)

## Maintenance
Limitations, future work, attention areas (from review)
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

Also delete `{feature}-manifest.toml` or archive if needed.

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
