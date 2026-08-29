---

name: deslop
description: Systematically reduce unnecessary complexity, duplication, abstractions, dependencies, files, and code in an existing project while preserving behavior and avoiding unnecessary refactoring. Use when an existing codebase has accumulated AI-generated or other implementation slop.

---

# DESLOP - Evidence-Based Complexity Reduction

## Mission

Reduce unnecessary complexity in an existing codebase **without changing its intended behavior**.

The goal is not to rewrite the project.

The goal is:

> **Make the existing system simpler, smaller, clearer, and easier to maintain while preserving its behavior.**

Assume the codebase may contain unnecessary work accumulated through previous implementations, particularly AI-generated code.

Be skeptical of existing complexity.

Do not assume that existing code is necessary merely because it works.

---

# Core Principles

## 1. Preserve behavior

Do not change externally observable behavior unless explicitly required.

Preserve:

* APIs
* Contracts
* Data formats
* Error behavior
* Side effects
* Configuration behavior
* Integration behavior
* Test behavior
* Backwards compatibility

When uncertain whether something is required, investigate before deleting it.

---

## 2. Evidence before deletion

Do not delete code simply because it looks unnecessary.

Before removing significant behavior, establish evidence through:

* Repository search
* Call-site analysis
* Reference analysis
* Tests
* Configuration inspection
* Dependency analysis
* Build tooling
* Runtime/integration usage where available
* Documentation
* Similar implementations

Distinguish between:

> **"I don't see why this exists."**

and:

> **"I have sufficient evidence that this is unnecessary."**

Only the second justifies deletion.

---

# Phase 0 - Establish a Baseline

Before modifying anything:

1. Inspect the repository structure.
2. Identify the project's build and test mechanisms.
3. Run the relevant existing validation.
4. Record the current baseline.
5. Understand the project's normal architecture and conventions.

Do not begin large-scale deletion before establishing that the project currently works.

If the existing test suite is incomplete or failing, record this explicitly.

Do not attribute pre-existing failures to your changes.

---

# Phase 1 - Inventory the Slop

Systematically inspect the project for unnecessary complexity.

Look for:

### Code

* Dead code
* Unreachable branches
* Duplicate implementations
* Duplicate helpers
* Redundant wrappers
* Needless indirection
* Overly generic utilities
* One-use abstractions
* Premature abstractions
* Speculative extensibility
* Excessive defensive programming
* Redundant validation
* Redundant error handling
* Boilerplate
* Needless conversions
* Needless transformations
* Needless state
* Unnecessary comments
* Excessive logging

### Architecture

* Abstractions with only one meaningful implementation
* Layers that provide no meaningful behavior
* Pass-through services
* Wrapper-on-wrapper designs
* Unnecessary factories
* Unnecessary adapters
* Unnecessary interfaces
* Excessive dependency injection
* Over-engineered configuration
* Frameworks or infrastructure used for trivial functionality

### Dependencies

Look for:

* Dependencies providing functionality already available elsewhere
* Dependencies used for trivial operations
* Duplicate libraries serving the same purpose
* Dependencies with extremely narrow usage
* Unused dependencies
* Transitive functionality that can replace custom code

Do not remove a dependency until all usage has been checked.

### Project Structure

Look for:

* Files with a single trivial purpose
* Duplicate resource/configuration files
* Empty or near-empty modules
* Redundant layers
* Generated artifacts accidentally committed
* Unused configuration
* Obsolete scripts
* Duplicate test infrastructure

---

# Phase 2 - Find the Highest-Confidence Wins

Do not randomly refactor.

Prioritize changes that are:

1. Clearly unnecessary
2. Easy to prove unnecessary
3. Low risk
4. High impact

Good candidates include:

* Unused code
* Duplicate code
* Unused dependencies
* Dead configuration
* Redundant wrappers
* Duplicate abstractions
* Clearly unreachable branches
* Obsolete compatibility code with proven no remaining consumers

Do not begin by redesigning the architecture.

---

# Phase 3 - One Reduction at a Time

Treat each deslop operation as an independent change.

For each candidate:

1. Identify what appears unnecessary.
2. Search all references.
3. Determine what behavior depends on it.
4. Determine why it can be removed or simplified.
5. Make the smallest possible change.
6. Run relevant validation.
7. Inspect the resulting diff.
8. Keep the change only if behavior remains correct.

Do not combine dozens of unrelated cleanup operations into one opaque change.

---

# Phase 4 - Remove Duplication

Search for multiple implementations of the same behavior.

When duplication exists:

1. Identify the canonical implementation.
2. Determine whether the other implementations can use it.
3. Remove unnecessary copies.
4. Preserve the existing behavior of callers.

Do not automatically create a new abstraction to eliminate duplication.

Prefer an existing suitable abstraction.

If no suitable abstraction exists, determine whether the duplication is actually harmful enough to justify introducing one.

**Two similar pieces of code are not automatically a reason to create an abstraction.**

---

# Phase 5 - Simplify Abstractions

For every abstraction that appears suspicious, ask:

> Does this abstraction provide meaningful behavior, or does it merely move code somewhere else?

Look for:

```text
caller
  ↓
wrapper
  ↓
service
  ↓
adapter
  ↓
helper
  ↓
actual operation
```

If several layers provide no meaningful behavior, determine whether they can safely be collapsed.

Prefer:

```text
caller
  ↓
meaningful abstraction
```

or, where appropriate:

```text
caller
  ↓
direct implementation
```

Do not collapse abstractions merely because they add lines.

Some abstractions exist for valid architectural, testing, ownership, or compatibility reasons.

---

# Phase 6 - Simplify Code

Look for code that can be made simpler while preserving behavior.

Examples:

* Multiple branches where one expression is sufficient
* Repeated transformations
* Redundant variables
* Unnecessary temporary values
* Duplicate validation
* Redundant condition checks
* Needless conversions
* Unnecessary wrappers
* Repeated configuration
* Custom implementations of standard functionality

Do not prioritize cleverness.

Prefer code that is:

* obvious
* idiomatic
* locally understandable
* consistent with the project
* easy to debug

---

# Phase 7 - Challenge Dependencies

For every dependency that appears unnecessary:

1. Find every usage.
2. Determine what functionality it provides.
3. Determine whether the project already has an equivalent capability.
4. Determine whether the standard library/framework/platform can replace it.
5. Assess compatibility and maintenance implications.
6. Remove it only when the evidence supports removal.

Do not replace a dependency merely to reduce dependency count.

The goal is reduced unnecessary complexity, not an arbitrary dependency count.

---

# Phase 8 - Tests Are Code Too

Apply the same scrutiny to tests.

Look for:

* Duplicate tests
* Duplicate fixtures
* Redundant setup
* Redundant teardown
* Overly abstract test helpers
* Helpers used only once
* Tests that verify implementation details unnecessarily
* Excessive mocking
* Boilerplate that existing test infrastructure already handles

Do not remove coverage merely because a test is verbose.

The objective is:

> **Maximum confidence with minimum unnecessary test complexity.**

---

# Phase 9 - AI Slop Detection

Be especially suspicious of code exhibiting these patterns:

### "Just in case" code

Code added for hypothetical future requirements.

### "Enterprise" abstraction

Multiple interfaces, factories, services, or adapters around trivial behavior.

### Wrapper proliferation

Functions that merely forward arguments without adding meaningful behavior.

### Configuration explosion

Configuration introduced where a constant or existing configuration mechanism would suffice.

### Defensive overengineering

Large amounts of validation or error handling without evidence that the scenarios are possible or required.

### Generic-for-no-reason code

Code generalized beyond the actual requirements.

### Duplicate helpers

New utilities that perform operations already provided elsewhere.

### AI-shaped comments

Comments explaining obvious code or describing what the code does instead of why it exists.

### Test ceremony

Large fixtures, mocks, helpers, or abstractions surrounding very small behaviors.

Treat these as **signals for investigation**, not automatic deletion criteria.

---

# Phase 10 - Architecture Review

Only after high-confidence local simplifications have been exhausted should you consider larger structural problems.

Ask:

> Is complexity repeated across the project because the architecture itself is unnecessarily complicated?

If yes, identify the smallest structural change that would materially reduce complexity.

Do not redesign the architecture simply because you prefer another architecture.

Architecture changes require evidence of a meaningful maintenance benefit.

---

# Phase 11 - Verify After Every Meaningful Reduction

After each meaningful reduction:

* Run relevant tests.
* Run static analysis.
* Run type checking where applicable.
* Run linting where applicable.
* Run the build where applicable.

For higher-risk changes, run broader integration/end-to-end validation.

Never perform a large deletion and assume the test suite will catch everything.

---

# Final Deslop Audit

Inspect the final diff.

For every deletion, ask:

> What evidence established that this was unnecessary?

For every retained abstraction, ask:

> What meaningful responsibility justifies its existence?

For every dependency, ask:

> What functionality does this dependency uniquely provide?

For every remaining layer, ask:

> Does this layer provide meaningful behavior or a documented architectural purpose?

For every remaining helper:

> Is this helper genuinely reusable or does it merely hide a small operation?

For every remaining configuration value:

> Is this actually configurable, or is configuration being used where it provides no value?

---

# Stop Conditions

Stop when further simplification would require:

* Guessing about behavior
* Changing public contracts
* Significant architectural redesign
* Broad behavioral changes
* Removing useful abstractions
* Sacrificing readability
* Sacrificing security
* Sacrificing maintainability
* Introducing more complexity than it removes

**Deslop is not a rewrite.**

The objective is not:

> "Make everything as small as possible."

The objective is:

> **"Remove complexity that cannot justify its existence."**

---

# Definition of Done

* [ ] Existing behavior is preserved.
* [ ] A working baseline was established.
* [ ] High-confidence unnecessary code was removed.
* [ ] Duplicate functionality was investigated.
* [ ] Unnecessary abstractions were challenged.
* [ ] Unnecessary dependencies were investigated.
* [ ] Tests were reviewed for unnecessary complexity.
* [ ] No unrelated feature work was introduced.
* [ ] Relevant tests pass.
* [ ] Required static analysis passes.
* [ ] Required builds pass.
* [ ] Every significant deletion has evidence behind it.
* [ ] The remaining complexity has a defensible reason to exist.
* [ ] The final diff is focused and understandable.
* [ ] – Em dashes and similar AI schenanigans are forbidden.

---

# Prime Directive

> **Do not clean code because it looks ugly.**
>
> **Find complexity that has no defensible reason to exist.**
>
> **Prove it is unnecessary.**
>
> **Delete it.**
>
> **Verify behavior.**
>
> **Repeat until further reduction is no longer worth the risk.**
