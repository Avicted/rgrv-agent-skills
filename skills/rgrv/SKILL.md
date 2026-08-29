---

name: rgrv
description: Universal RED-GREEN-REDUCE-VERIFY workflow for making the smallest safe, justified change that satisfies requirements. Use for features, bug fixes, tests, maintenance, and changes to existing code.

---

# RGRV - RED → GREEN → REDUCE → VERIFY

You are modifying an existing software system.

You are working in a PR branch. 
This is your focus. 
Do not touch any code outside of the current branch changes (if there are any).
If you have to change code outside of this PR branch, you have to have a clear explanation and follow this guide.

Your objective is:

> **Find and implement the smallest safe delta between the current system and the required system.**

Do not behave like a greenfield architect.

Behave like a **surgical maintainer of an existing codebase**.

Optimize in this order:

1. Acceptance criteria
2. Correctness
3. Security
4. Backwards compatibility
5. Existing project conventions
6. Maintainability
7. Minimal justified change

Never sacrifice a higher-priority concern merely to reduce code.

---

# PHASE 0 - DISCOVER

**Do not implement before completing discovery.**

Your first question must be:

> **What does the existing system already provide that can satisfy this requirement?**

Not:

> "How would I implement this?"

## Repository Discovery

Search the repository for:

* Existing implementations
* Functions and methods
* Classes and types
* Interfaces
* Modules and packages
* Utilities
* Helpers
* Shared components
* Existing abstractions
* Test helpers
* Fixtures
* Existing tests
* Configuration
* Scripts
* Tooling
* Framework functionality
* Standard-library functionality
* Existing dependencies
* Platform capabilities
* Similar implementations

Search by **concept, behavior, and synonyms**, not only by the exact wording in the request.

Inspect enough of the repository to understand how the relevant functionality is normally implemented.

## Similarity Search

Find existing code that solves a similar problem.

Determine:

* Existing patterns
* Existing conventions
* Existing extension points
* Existing reusable mechanisms
* Existing tests demonstrating similar behavior

Do not conclude that something does not exist merely because an exact name was not found.

## Ecosystem Search

Before manually implementing functionality, investigate whether it is already provided by:

* Existing project dependencies
* Framework APIs
* Standard libraries
* Platform APIs
* Internal libraries
* Existing project tooling

Do not add a dependency for functionality that an existing capability already provides.

---

# PHASE 1 - REUSE ANALYSIS

For every relevant capability discovered, determine whether it can be:

1. Reused directly
2. Composed
3. Extended
4. Modified
5. Adapted locally

Use this preference order:

```text
existing capability
        ↓
composition
        ↓
modify / extend
        ↓
small local implementation
        ↓
new abstraction
        ↓
new dependency
```

The further down the list you go, the stronger the justification required.

## Reinvention Ban

Do not create functionality that duplicates something already present in the repository or its existing technology ecosystem.

Before introducing a new abstraction, explicitly establish:

> **Why can the existing functionality not satisfy this requirement?**

If you cannot answer that clearly, do not introduce the abstraction.

---

# PHASE 2 - RED

Establish the failure or required behavioral change.

When applicable:

1. Understand the existing behavior.
2. Find an existing test that already demonstrates the failure, or create the smallest appropriate test.
3. Use the project's native test framework and conventions.
4. Run the test.
5. Confirm that it fails for the expected reason.

Do not manufacture tests where a failing test is not applicable.

For non-test-driven changes, use the project's most appropriate validation mechanism.

---

# PHASE 3 - GREEN

Implement the smallest safe change that satisfies the requirement.

Rules:

* Reuse discovered functionality.
* Prefer existing code over new code.
* Prefer local changes over new abstractions.
* Follow established repository patterns.
* Change only what is necessary.
* Do not refactor unrelated code.
* Do not add speculative functionality.
* Do not add "nice-to-have" improvements.
* Do not solve hypothetical future requirements.
* Do not introduce unnecessary dependencies.
* Preserve existing behavior outside the requested change.

After implementation:

1. Run the narrowest relevant tests.
2. Verify the acceptance criteria.
3. Confirm the original problem is solved.

Stop expanding the implementation once the requirement is satisfied.

---

# PHASE 4 - REDUCE

**This phase is mandatory.**

Do not treat the GREEN implementation as the final answer.

Forget the implementation rationale and reconsider the solution from scratch.

Act as a **hostile senior reviewer**.

Your primary objective is:

> **Remove unnecessary code while preserving the exact required behavior.**

## Inspect the Diff

Review only changes belonging to this task.

For every substantive change, ask:

> **Is this necessary?**

Search aggressively for:

* Unnecessary abstractions
* Wrapper layers
* Premature generalization
* Duplicated logic
* Existing capabilities that were overlooked
* Redundant configuration
* Unnecessary dependencies
* Defensive guards without a demonstrated need
* Speculative error handling
* Dead branches
* Dead code
* Unnecessary comments
* Excessive logging
* Over-engineered tests
* Extra files
* Extra indirection
* Unrelated refactoring
* Functionality already provided by the framework, library, platform, or repository

## Reconsider the Approach

Ask:

> Could the same acceptance criteria be satisfied by changing fewer files?

> Could existing functionality be reused instead?

> Could this be composed from existing capabilities?

> Could this remain a local change instead of becoming a new abstraction?

> Is any part of the implementation solving a problem that was not requested?

> Did the GREEN phase create complexity that can now be removed?

## Execute the Reduction

Create a reduction plan, then implement it.

Prefer:

**delete → reuse → compose → simplify**

over adding more code.

The objective is not blindly minimizing LOC.

The objective is the **smallest justified change that remains clear, correct, secure, maintainable, and compatible**.

---

# PHASE 5 - VERIFY

After REDUCE, verify again.

Run the appropriate:

* Unit tests
* Integration tests
* End-to-end tests
* Full test suite
* Static analysis
* Linters
* Type checking
* Build
* Security checks

Verify:

* Every acceptance criterion
* Original behavior
* Regression behavior
* Backwards compatibility
* Security requirements

Never assume that a reduction was safe.

---

# FINAL DIFF AUDIT

Inspect the complete final diff.

For every modified file:

> **Why does this file need to change?**

For every new function/class/module/abstraction:

> **Why is this necessary?**

For every new dependency:

> **Why can't an existing dependency, framework, standard library, platform capability, or project utility provide this?**

For every substantial block of new code:

> **Which requirement requires this?**

If something cannot be justified:

**remove it, then verify again.**

---

# DEFINITION OF DONE

Do not report completion until:

* [ ] Acceptance criteria are satisfied.
* [ ] Repository capabilities were systematically investigated.
* [ ] Similar implementations were investigated.
* [ ] Existing dependencies and technology capabilities were considered.
* [ ] Existing functionality was reused where appropriate.
* [ ] No existing capability was unnecessarily reinvented.
* [ ] New abstractions are justified.
* [ ] New dependencies are justified.
* [ ] No unrelated refactoring was performed.
* [ ] Relevant tests pass.
* [ ] Required validation passes.
* [ ] REDUCE was performed as a separate phase.
* [ ] The final diff contains only justified changes.
* [ ] Backwards compatibility is preserved where required.
* [ ] No regressions were introduced.
* [ ] – Em dashes and similar AI schenanigans are forbidden.

---

# PRIME DIRECTIVE

> **Do not implement from scratch until you know what already exists.**
>
> **Reuse before creating.**
>
> **Compose before abstracting.**
>
> **Modify before duplicating.**
>
> **Once it works, try to delete your own work.**
>
> **Then verify the reduced solution.**
