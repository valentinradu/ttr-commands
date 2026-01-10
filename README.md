# AI-Assisted Development Workflow Commands

Progressive constraint workflow based on "The Thousand Token Rule" course principles.

## Philosophy

Good software practices work but are expensive to follow. AI makes iteration on planning documents nearly free, enabling quality through cheap constraint before expensive implementation.

**Core workflow:** Plan → Design → Test → Implement → Verify → Consolidate

Each phase produces artifacts that constrain the next. By implementation, the problem space is so narrow that correctness becomes likely rather than lucky.

## Commands

| Command | Purpose | When to use |
|---------|---------|-------------|
| `/plan` | Requirements → technical → test planning | Start of new feature |
| `/stub` | Generate interface stubs from plans | After planning complete |
| `/implement` | TDD cycle: test → implement → commit | After stubs exist |
| `/audit` | Context-aware quality checks | After implementation or during planning |
| `/review` | AI-assisted human review | Before merge |
| `/consolidate` | Create permanent docs, clean up artifacts | After feature merged |

## Workflow

### 1. Planning phase

```bash
/plan feature-name
```

Guides through three planning stages with validation gates:

**Requirements** (`feature-name-req.md`):
- What problem this solves
- System behavior, constraints, acceptance criteria
- No technical solutions yet
- Target: 600-1000 tokens

**Technical** (`feature-name-tech.md`):
- Implementation approach
- Dependencies, architecture, interfaces
- Failure modes and risks
- Target: 400-800 tokens

**Tests** (`feature-name-tests.md`):
- What to test, edge cases, coverage goals
- Maps to technical components
- Enumerates failure scenarios
- Target: 300-600 tokens per component

Each stage validates before progressing. Checks for incomplete documents.

### 2. Design phase

```bash
/stub feature-name
```

Generates interface stubs from planning:

- Function signatures with exact types
- Documentation from requirements
- Precondition/postcondition assertions from tests
- `todo!()` bodies as placeholders

Must compile (`cargo check` passes). Becomes contract for implementation.

### 3. Implementation phase

```bash
/implement feature-name
/implement feature-name function-name
```

TDD cycle for each function:

1. Verify test exists (test-first mandatory)
2. Run tests (confirm red)
3. Implement minimal code to pass
4. Run tests (confirm green)
5. Commit with descriptive message
6. Next function

Small commits enable bisect, minimal reverts, focused review.

Context management: Provide only function, tests, direct dependencies. Enables cheaper models.

### 4. Verification phase

```bash
/audit [natural language description]
/audit security
/audit performance on http handlers
/audit consistency
```

Context-aware quality checks:

**On planning docs:** Completeness, clarity, consistency
**On code:** Security, performance, correctness, maintainability
**On both:** Plans vs implementation consistency

**Mandatory test-first fix workflow:**
1. Write test reproducing issue (must fail)
2. Verify test fails
3. Implement fix
4. Verify test passes
5. Commit test + fix together

Never fix without reproducing test.

### 5. Review phase

```bash
/review feature-name
/review feature-name --scope file.rs
```

AI-assisted human review:

1. Generate pre-review explanation (what changed, why, risks)
2. Human reads code with explanation as context
3. If explanation doesn't match code → bug found
4. Collect feedback, implement changes
5. Re-audit, re-review until approved

Auto-approve criteria for cosmetic changes with high confidence.

### 6. Consolidation phase

```bash
/consolidate feature-name
```

After feature complete:

1. Create permanent documentation (`{feature}.md`)
   - Purpose, architecture, key decisions
   - Usage, testing strategy, maintenance notes
   - Target: 400-800 tokens
2. Delete planning artifacts (with confirmation)
   - Preserved in git history
   - Key information captured in consolidated doc
3. Optional: Create ADR for architectural decisions

Leaves repository clean for next feature.

## Configuration

### Docs location

First use of `/plan` asks:

```
Where should planning docs be stored?
- /docs (recommended)
- Custom path
```

Saves to `.psiar-config.json`:

```json
{
  "docsPath": "/docs"
}
```

Feature docs created at `{docsPath}/{feature}/`. Change anytime by editing config file.

## Key Principles

### The thousand token rule

No iterable task should exceed ~1,000 tokens.

**Why:** 10 iterations on 1,000 token document = 10,000 tokens. Same feature as code (3,000-5,000 tokens) iterated 10 times = 30,000-50,000 tokens. Document iteration costs 1/5 as much.

Decomposition that keeps tasks under 1,000 tokens is the same decomposition that makes good software.

### Test-first mandatory

Every fix must have reproducing test first:

- Audit finding → Test reproducing issue → Fix
- Bug report → Test reproducing bug → Fix
- Code review feedback → Test demonstrating problem → Fix

If you can't write failing test, you don't understand problem well enough to fix it.

### Progressive constraint

Each phase narrows possibilities:

- Requirements: Problem space defined
- Technical: Solution space narrowed
- Tests: Behavior specified
- Stubs: Interfaces locked
- Implementation: Fill in the blanks

By implementation, there's one obvious way to make tests pass.

### Commit rhythm

Small, frequent commits after each validated step:

- Refined requirements → Commit
- Completed technical plan → Commit
- Implemented function + tests pass → Commit

Benefits: Easy bisect, minimal revert cost, coherent review.

### Model routing

More constraint = less reasoning = cheaper model:

| Phase | Model tier | Why |
|-------|-----------|-----|
| Requirements refinement | Frontier | Low constraint, high reasoning |
| Technical planning | Frontier | Architecture decisions |
| Test planning | Frontier | Systematic thinking |
| Stub generation | Mid-tier | Following template |
| Test implementation | Mid-tier | Clear spec from plans |
| Implementation | Low-tier | High constraint from tests |

If 60% of tokens are implementation, routing to 30x cheaper models saves substantially.

## File Structure

Typical feature during development:

```
/docs/feature-name/
├── feature-name-req.md      # Requirements
├── feature-name-tech.md     # Technical plan
├── feature-name-tests.md    # Test plan
├── feature-name-review.md   # Review notes
└── feature-name-audit.md    # Audit findings

/src/
├── feature-name.rs           # Implementation
└── tests/
    └── feature-name_test.rs  # Tests
```

After consolidation:

```
/docs/
└── feature-name.md           # Permanent documentation

/src/
├── feature-name.rs           # Implementation
└── tests/
    └── feature-name_test.rs  # Tests
```

Planning directory `/docs/feature-name/` deleted, preserved in git history.

## Examples

### New feature workflow

```bash
# Planning
/plan user-authentication
# → Creates req.md, tech.md, tests.md with validation gates

# Design
/stub user-authentication
# → Generates interface stubs

# Implementation
/implement user-authentication
# → TDD cycle through each function

# Verification
/audit security
/audit performance
# → Finds issues, fixes test-first

# Review
/review user-authentication
# → Pre-review explanation + guided human review

# Consolidation
/consolidate user-authentication
# → Permanent doc, cleanup artifacts
```

### Bug fix workflow

```bash
# Reproduce
# Write test demonstrating bug (test fails)

# Audit investigation
/audit error-handling in auth module
# → Identifies root cause

# Fix (test-first already done)
/implement user-authentication validate_token
# → Make test pass

# Verify
/audit security on validate_token
# → Ensure fix is secure

# Review
/review user-authentication --scope validate_token
# → Quick targeted review
```

### Refactoring workflow

```bash
# Document current behavior
/plan refactor-data-layer
# → Requirements: preserve existing behavior
# → Technical: new architecture
# → Tests: comprehensive coverage of current behavior

# Implement
/stub refactor-data-layer
/implement refactor-data-layer
# → TDD cycle, tests ensure no behavior change

# Verify
/audit consistency
# → Confirm behavior unchanged

/consolidate refactor-data-layer
# → Document architectural decision
```

## Integration with Git

Commands assume git repository:

- Commits after each validated step
- Feature branches encouraged
- Git history preserves deleted planning docs
- Commit messages reference planning docs

## Tool Agnostic

Examples assume:
- Rust (for examples in course)
- Claude AI (course default)

Principles apply to any language with good tooling:
- TypeScript with strict types
- Go with staticcheck
- Python with mypy

And any AI coding assistant:
- Claude, GPT-4, local models
- Important: Model routing by constraint level

## Cost Optimization

Front-loading work in documents enables:

1. **Cheap iteration:** 10x cheaper than iterating on code
2. **Model routing:** 60%+ tokens at low tier for implementation
3. **Defect prevention:** 1x cost in requirements vs 6.5x in code, 100x in production

Example project:
- Planning: 5,000 tokens @ $15/M = $0.075
- Implementation: 20,000 tokens @ $1/M = $0.020 (routed to low-tier)
- **Total: $0.095 vs $0.60 without workflow (6x savings)**

## Getting Started

1. Copy command files to your agent configuration directory
2. Run `/plan feature-name` for first feature
3. Follow prompts through entire workflow
4. Adjust docs location and model routing for your needs

## Further Reading

Based on course: "The Thousand Token Rule"

Core concepts:
- Front-load decisions into cheap iterations
- Progressive constraint workflow
- Test-first mandatory
- Machines checking machines
- Review by exception
