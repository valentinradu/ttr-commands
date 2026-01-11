# AI-Assisted Development Workflow Commands

A structured workflow for building features with AI assistance, based on [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule).

Instead of asking AI to "implement feature X" and getting unpredictable results, this workflow guides you through progressive phases where each step constrains and validates the next.

**Plan → Design → Test → Stub → Implement → Audit → Fix → Review → Consolidate**

By the time you reach implementation, most decisions are already made and tested. You're filling in the blanks, not guessing.

## Why This Approach?

**The Problem**
Asking AI to implement features directly often produces code that "looks right" but has subtle bugs, wrong assumptions, or doesn't match your architecture.

**The Solution**
Break the work into phases. Iterate on lightweight documents first (requirements, technical design, test plans) before writing any code. Each phase catches mistakes early when they're cheap to fix.

## Commands

| Command | Description |
|---------|-------------|
| `/ttr-init` | Resume workflow session |
| `/ttr-plan` | Requirements → technical → tests |
| `/ttr-stub` | Generate interface stubs |
| `/ttr-implement` | TDD cycle per function |
| `/ttr-audit` | Context-aware quality checks |
| `/ttr-fix` | Fix all issues test-first |
| `/ttr-review` | AI-assisted code review |
| `/ttr-consolidate` | Permanent docs, cleanup |

## How It Works

Each command builds on the previous one in a specific order:

### 1. Plan → `/ttr-plan feature-name`
Start here. Define requirements, create technical design, write test plan. All documents, no code yet. Iterate until it's clear what you're building.

### 2. Stub → `/ttr-stub feature-name`
Generate empty function signatures and interfaces from your design. Creates the skeleton your tests will use.

### 3. Implement → `/ttr-implement feature-name`
Fill in the implementation, one function at a time using TDD. Tests already exist, you're making them pass.

### 4. Audit → `/ttr-audit security`
Run automated checks for security, performance, correctness issues. Generates a report of what needs fixing.

### 5. Fix → `/ttr-fix`
Fix all reported issues. Requires writing a failing test for each issue before fixing it.

### 6. Review → `/ttr-review feature-name`
Prepare for code review. AI highlights potential concerns for human reviewers.

### 7. Consolidate → `/ttr-consolidate feature-name`
Clean up temporary docs, update permanent documentation, final polish.

---

**Resume anytime:** `/ttr-init` detects where you left off and loads the context to continue.

## Key Principles

### Iterate on documents, not code
Fixing a requirements doc is faster and cheaper than fixing implemented code. Get the design right before typing a single line of implementation.

### Test-first is mandatory
Every fix needs a failing test first. If you can't write the failing test, you don't understand the problem well enough to fix it.

### Progressive constraint
Each phase narrows your options. By the time you write code, there's only one obvious way forward that satisfies all constraints.

## Configuration

The first time you run `/ttr-plan`, you'll be asked where to store workflow documents. Your preference is saved to `.ttr.toml`:

```toml
docsPath = "/docs"
```

The workflow automatically tracks changed files using manifest files:
- `{feature}-stub-manifest.json`
- `{feature}-impl-manifest.json`

## Learn More

This workflow is based on [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule) course. The course provides deeper insight into the principles, but you don't need to take it to use these commands effectively.
