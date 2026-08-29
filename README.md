# RGRV Agent Skills

Two language-agnostic Agent Skills for keeping AI-generated code **small, reusable, and maintainable**.

## The problem

AI agents are very good at making code work.

They're much less good at asking:

> **"Does this code need to exist?"**

They tend to reinvent existing functionality, add unnecessary abstractions, over-engineer tests, and turn small changes into large diffs.

These skills are designed to counter that.

## Skills

### RGRV

**RED → GREEN → REDUCE → VERIFY**

Use when implementing or changing something.

```text
DISCOVER → REUSE → RED → GREEN → REDUCE → VERIFY
```

The agent must first look for existing code, abstractions, libraries, framework features, test resources, and other capabilities that can be reused.

After getting the solution working, it actively tries to **remove its own unnecessary work**.

---

### DESLOP

In the spirit of Uncle Bob's (Robert C. Martin) CRAP (Change Risk Anti-Pattern) metric.

Use when the project has **already accumulated slop**.

It looks for:

* Duplicate code
* Unnecessary abstractions
* Wrapper layers
* Dead code
* Unused dependencies
* Speculative/defensive code
* Unnecessary configuration
* Over-engineered tests
* AI-generated complexity

It is not a license to rewrite the project.

**Evidence before deletion.**

## The goal

Not:

> **Minimum lines of code.**

But:

> **The smallest safe, justified change that satisfies the requirements.**

The workflow:

```text
Discover what already exists.
        ↓
Reuse it.
        ↓
Make the smallest change.
        ↓
Make it work.
        ↓
Try to delete your own work.
        ↓
Verify.
```

## Installation

Copy the `skills/rgrv` folder into your agent's `skills` folder.

## Usage

For implementation work:

```text
/rgrv
```

For cleaning up accumulated complexity:

```text
/deslop
```

The skills are intentionally **language- and framework-agnostic**. They rely on the agent discovering and using the capabilities already present in the target project.

## Philosophy

> **Discover before implementing.**
> **Reuse before reinventing.**
> **Compose before abstracting.**
> **Make the smallest safe change.**
> **Delete what you don't need.**
> **Verify everything.**
