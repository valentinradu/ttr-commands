# ttr-init

Resume workflow. Detect phase and load context.

## Detection

1. Load `.ttr.toml` (docsPath)
2. Find manifests: `{feature}-stub-manifest.json`, `{feature}-impl-manifest.json`
3. Find docs: `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`, `{feature}-audit.md`, `{feature}-review.md`

## Phase

- No config → `/ttr-plan`
- Req only → Continue technical
- Req + tech → Continue tests
- Docs complete, no stubs → `/ttr-stub`
- Stubs done, no impl → `/ttr-implement`
- Impl done, no audit → `/ttr-audit`
- Audit with issues → `/ttr-fix`
- Audit clean, no review → `/ttr-review`
- Review done → `/ttr-consolidate`

## Context

Summarize (100-200 tokens): requirements, approach, test strategy, modified files, functions, current state, issues.

`AskUserQuestion: "Detected {phase}. Context: {summary}. Continue/override/specify?"`

## Usage

```bash
/ttr-init
/ttr-init feature-name
```
