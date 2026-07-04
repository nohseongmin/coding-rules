# Contributing to coding-rules

This is a living standard. Rules should earn their place: **a rule belongs here only if the cost of following it is less than the cost of the bug or mess it prevents.**

## Proposing a change

1. **Open an issue first** describing the rule you want to add, change, or remove, and *why* (the concrete problem it addresses). Real examples beat opinions.
2. For wording/example/typo fixes, a direct PR is fine.
3. Every PR must explain the **why** in its description, not just the what.

## What makes a good rule

- **Actionable** — a reader can tell whether they followed it.
- **Language-agnostic** — or clearly scoped if language-specific.
- **Justified** — pairs a WHY with the rule; ideally a BAD/GOOD example.
- **Non-redundant** — doesn't restate an existing rule.

## Versioning

We follow [SemVer](https://semver.org/)-style intent for the standard:

- **MAJOR** — a rule is removed or reversed (existing code may need to change).
- **MINOR** — a new rule or section is added.
- **PATCH** — clarifications, examples, typos.

Update [CHANGELOG.md](CHANGELOG.md) in the same PR, under an `## [Unreleased]` heading.

## Style

Rules use the format **RULE** (imperative) → **WHY** → optional **BAD / GOOD**. Keep it terse; this is a reference, not an essay.
