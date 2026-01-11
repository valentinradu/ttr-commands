# AI-Assisted Development Workflow Commands

A structured workflow for building features with AI assistance, based on [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule).

Instead of asking AI to "implement feature X" and getting unpredictable results, this workflow guides you through progressive phases where each step constrains and validates the next.

**Plan → Stub → Implement → Audit → Fix → Review → Consolidate**

By the time you reach implementation, most decisions are already made and tested. You're filling in the blanks, not guessing.

## Why This Approach?

**The Problem**
Asking AI to implement features directly often produces code that "looks right" but has subtle bugs, wrong assumptions, or doesn't match your architecture.

**The Solution**
Break the work into phases. Iterate on lightweight documents first (spec, test plans) before writing any code. Each phase catches mistakes early when they're cheap to fix.

## Commands

| Command | Description |
|---------|-------------|
| `/ttr-scan` | Scan codebase, report state, sync manifest |
| `/ttr-plan` | Create spec and test plan |
| `/ttr-stub` | Generate interface stubs |
| `/ttr-implement` | TDD cycle per function |
| `/ttr-audit` | Context-aware quality checks |
| `/ttr-fix` | Fix all issues test-first |
| `/ttr-review` | AI-assisted code review (trivial/non-trivial split) |
| `/ttr-consolidate` | Permanent docs, cleanup |

## How It Works

### 1. Plan → `/ttr-plan feature-name`
Start here. Create a unified spec (requirements + design) and test plan. All documents, no code yet.

### 2. Stub → `/ttr-stub feature-name`
Generate empty function signatures and interfaces from your spec. Creates the skeleton your tests will use.

### 3. Implement → `/ttr-implement feature-name`
Fill in the implementation, one function at a time using TDD. Tests already exist, you're making them pass.

### 4. Audit → `/ttr-audit security`
Run automated checks for security, performance, correctness issues. Adds findings to manifest.

### 5. Fix → `/ttr-fix`
Fix all reported issues. Requires writing a failing test for each issue before fixing it.

### 6. Review → `/ttr-review feature-name`
Splits changes into trivial (format, naming) and non-trivial (logic, architecture). Offers to commit trivial changes immediately.

### 7. Consolidate → `/ttr-consolidate feature-name`
Clean up temporary docs, update permanent documentation, final polish.

---

**Scan anytime:** `/ttr-scan` detects TODO comments in code, syncs the manifest, and reports current state.

## Key Principles

### Iterate on documents, not code
Fixing a spec is faster and cheaper than fixing implemented code. Get the design right before typing a single line of implementation.

### Test-first is mandatory
Every fix needs a failing test first. If you can't write the failing test, you don't understand the problem well enough to fix it.

### Progressive constraint
Each phase narrows your options. By the time you write code, there's only one obvious way forward that satisfies all constraints.

## Configuration

The first time you run `/ttr-plan`, you'll be asked where to store workflow documents. Your preference is saved to `.ttr.toml`:

```toml
docsPath = "/docs"
```

## Manifest

Each feature tracks progress in `{feature}-manifest.toml`:

```toml
[[tasks]]
name = "Implement parse_config"
status = "done"  # todo | in-progress | done
locations = ["src/config.rs:42", "tests/config_test.rs:15"]

[[tasks]]
name = "Fix validation bug"
status = "todo"
locations = ["src/input.rs:87"]
```

The manifest syncs with TODO comments in code via `/ttr-scan`.

## Learn More

This workflow is based on [The Thousand Token Rule](https://www.swiftcraft.io/courses/the-thousand-token-rule) course. The course provides deeper insight into the principles, but you don't need to take it to use these commands effectively.
