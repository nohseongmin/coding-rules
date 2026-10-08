# Coding Rules

Language-independent standards for maintainable code, including code written with AI assistants. The rules focus on hardcoded configuration, inconsistent implementation, and security defects.

## Files

| File | Purpose |
|---|---|
| [RULES.md](RULES.md) | Full standard: design, naming, configuration, security, errors, refactoring, testing, architecture, version control, and execution |
| [checklists/PRE_COMMIT.md](checklists/PRE_COMMIT.md) | Short pre-commit checklist |
| [.editorconfig](.editorconfig) | Basic formatting across editors |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to propose rule changes |
| [CHANGELOG.md](CHANGELOG.md) | Version history |

## Core rules

1. Understand the code before changing it.
2. Follow the codebase's existing conventions.
3. Use configuration or arguments for values that vary.
4. Keep secrets and environment-specific endpoints out of source.
5. Make the smallest change that meets the requirement.
6. Handle errors explicitly.
7. Clean up small issues in code you touch.
8. Ask when a requirement is ambiguous.

## Using the standard

For global Claude Code instructions, put the condensed rules in your user instructions and keep RULES.md as a deeper reference:

```text
~/.claude/CLAUDE.md        Condensed instructions, loaded automatically
~/.claude/CODING_RULES.md  Full RULES.md reference
```

For a repository, add RULES.md at its root and reference it from the project's CLAUDE.md or CONTRIBUTING.md.

For manual use, read [RULES.md](RULES.md) and use [the checklist](checklists/PRE_COMMIT.md) before committing.

## Versioning

Major versions remove or reverse rules. Minor versions add rules or sections. Patch versions clarify wording, examples, or typos.

## Credits

The core standard is original. Part 10, Goal-Driven Execution, and the surgical-change guidance in P0-5 and P0-7 were inspired by Andrej Karpathy's observations as collected in [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

## License

MIT.
