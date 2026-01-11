# AI-Assisted Development Workflow Commands

A structured workflow for building features with AI assistance. Instead of asking AI to "implement feature X" and getting unpredictable results, this workflow guides you through progressive phases where each step constrains and validates the next.

**The idea:** Plan what to build → Design interfaces → Write tests → Generate stubs → Implement → Fix issues → Review → Document

By the time you reach implementation, most decisions are already made and tested. You're filling in the blanks, not guessing.

## Why This Approach?

**Problem:** Asking AI to implement features directly often produces code that "looks right" but has subtle bugs, wrong assumptions, or doesn't match your architecture.

**Solution:** Break the work into phases. Iterate on lightweight documents first (requirements, technical design, test plans) before writing any code. Each phase catches mistakes early when they're cheap to fix.

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

The workflow follows a specific order. Each command builds on the previous one:

1. **`/ttr-plan feature-name`** - Start here. Define requirements, create technical design, write test plan. All documents, no code yet. Iterate until it's clear what you're building.

2. **`/ttr-stub feature-name`** - Generate empty function signatures and interfaces from your design. Creates the skeleton your tests will use.

3. **`/ttr-implement feature-name`** - Fill in the implementation, one function at a time using TDD. Tests already exist, you're making them pass.

4. **`/ttr-audit security`** - Run automated checks for security, performance, correctness issues. Generates a report of what needs fixing.

5. **`/ttr-fix`** - Fix all reported issues. Requires writing a failing test for each issue before fixing it.

6. **`/ttr-review feature-name`** - Prepare for code review. AI highlights potential concerns for human reviewers.

7. **`/ttr-consolidate feature-name`** - Clean up temporary docs, update permanent documentation, final polish.

**Resume anytime:** `/ttr-init` detects where you left off and loads the context to continue.

## Key Principles

**Iterate on documents, not code** - Fixing a requirements doc is faster and cheaper than fixing implemented code. Get the design right before typing a single line of implementation.

**Test-first is mandatory** - Every fix needs a failing test first. If you can't write the failing test, you don't understand the problem well enough to fix it.

**Progressive constraint** - Each phase narrows your options. By the time you write code, there's only one obvious way forward that satisfies all constraints.

**Commit frequently** - Small commits after each validated step make it easy to review, bisect bugs, and revert if needed.

## Configuration

The first time you run `/ttr-plan`, it will ask where to store workflow documents and save your preference to `.ttr.toml`:

```toml
docsPath = "/docs"
```

The workflow tracks changed files automatically using manifest files like `{feature}-stub-manifest.json` and `{feature}-impl-manifest.json`.

## Learn More

This workflow is based on principles from [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule) course, but you don't need to take the course to use these commands effectively.
