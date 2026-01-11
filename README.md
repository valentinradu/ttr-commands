# AI-Assisted Development Workflow Commands

Progressive constraint workflow based on [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule) course.

**Core workflow:** Plan → Design → Test → Implement → Verify → Consolidate

Each phase constrains the next. By implementation, correctness becomes likely rather than lucky.

## Commands

| Command | Description |
|---------|-------------|
| `/ttr:init` | Resume workflow session |
| `/ttr:plan` | Requirements → technical → tests |
| `/ttr:stub` | Generate interface stubs |
| `/ttr:implement` | TDD cycle per function |
| `/ttr:audit` | Context-aware quality checks |
| `/ttr:fix` | Fix all issues test-first |
| `/ttr:review` | AI-assisted code review |
| `/ttr:consolidate` | Permanent docs, cleanup |

## Quick Start

```bash
/ttr:plan feature-name    # Requirements → technical → tests
/ttr:stub feature-name    # Generate interface stubs
/ttr:implement feature-name    # TDD cycle per function
/ttr:audit security       # Find issues
/ttr:fix                  # Fix all issues test-first
/ttr:review feature-name  # Human review
/ttr:consolidate feature-name    # Permanent docs, cleanup
```

Resume session:
```bash
/ttr:init    # Detect phase, load context
```

## Key Principles

**The thousand token rule:** 10 iterations on 1,000 token document = 10,000 tokens. Same feature as code iterated 10 times = 30,000-50,000 tokens. Document iteration costs 1/5 as much.

**Test-first mandatory:** Every fix requires failing test first. If you can't write failing test, you don't understand the problem well enough to fix it.

**Progressive constraint:** Requirements → Technical → Tests → Stubs → Implementation. Each phase narrows possibilities until there's one obvious way forward.

**Commit rhythm:** Small commits after each validated step. Easy bisect, minimal reverts, coherent review.

## Configuration

First `/ttr:plan` prompts for docs location. Saves to `.psiar-config.json`:

```json
{"docsPath": "/docs"}
```

Manifests track changed files: `{feature}-stub-manifest.json`, `{feature}-impl-manifest.json`

## Learn More

Complete course: [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule)
