# ttr-init

Resume workflow session. Detect current phase and load context.

## Detection logic

Check in order:
1. `.psiar-config.json` exists? Load docsPath
2. Manifests exist? `{feature}-stub-manifest.json`, `{feature}-impl-manifest.json`
3. Docs exist? `{feature}-req.md`, `{feature}-tech.md`, `{feature}-tests.md`, `{feature}-audit.md`, `{feature}-review.md`
4. Implementation files exist? Check manifest file lists

## Phase detection

- No config → First time, run `/ttr-plan`
- Config + no docs → Run `/ttr-plan {feature}`
- Req doc only → Continue with technical plan
- Req + tech docs → Continue with test plan
- All planning docs, no stubs → Run `/ttr-stub {feature}`
- Stub manifest exists, no impl manifest → Run `/ttr-implement {feature}`
- Impl manifest exists, no audit → Run `/ttr-audit`
- Audit exists, issues found → Run `/ttr-fix`
- Audit clean, no review → Run `/ttr-review {feature}`
- Review approved → Run `/ttr-consolidate {feature}`

## Context loading

Load and summarize (100-200 tokens):
- Planning docs: Key requirements, approach, test strategy
- Manifests: Files modified, functions implemented
- Implementation files from manifests: Current state
- Audit findings: Unresolved issues

Present: `AskUserQuestion: "Detected phase: {phase}. Context: {summary}. Continue/override/specify feature?"`

## Usage

```bash
/ttr-init
/ttr-init feature-name
```
