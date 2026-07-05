# coding-rules

> Language-agnostic coding standards that guard against the **3 pains of AI / "vibe" coding** — hardcoding, inconsistency, and security holes.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.1.0-brightgreen.svg)](CHANGELOG.md)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-orange.svg)](CONTRIBUTING.md)

---

## Why this exists

AI-assisted coding is fast, but speed without guardrails produces three recurring failures:

1. **Hardcoding** — secrets, URLs, and magic numbers baked straight into source.
2. **Inconsistency** — every file written in a different style, impossible to maintain.
3. **Security holes** — no input validation, string-built SQL, leaked stack traces.

This repo is a **single, enforceable standard** that turns "whatever works right now" into code that stays maintainable, refactorable, and safe — whether a human or an AI wrote it.

## What's inside

| File | Purpose |
|------|---------|
| **[RULES.md](RULES.md)** | The full standard: Prime Directives, SOLID/DRY/KISS/YAGNI, Clean Code, No-Hardcoding, Security (OWASP), Errors, Refactoring, Testing, Architecture, VCS, Goal-Driven Execution. |
| **[checklists/PRE_COMMIT.md](checklists/PRE_COMMIT.md)** | 30-second pre-commit gate. |
| **[.editorconfig](.editorconfig)** | Enforces basic formatting consistency across editors. |
| **[CONTRIBUTING.md](CONTRIBUTING.md)** | How to propose changes to the rules. |
| **[CHANGELOG.md](CHANGELOG.md)** | Versioned history of the standard. |

## The 8 Prime Directives (highest priority)

Everything else is detail. If you only remember these, you avoid most of the damage:

1. **Understand before you write.** If you can't explain why code works, don't ship it.
2. **Match the codebase.** Consistency beats personal taste.
3. **No silent hardcoding.** If a value varies by env/user/secret/time → constant, config, or argument.
4. **Never invent secrets or endpoints.** Use env/config placeholders.
5. **Smallest change that works.** Propose out-of-scope refactors separately.
6. **Fail loudly, don't swallow errors.**
7. **Leave it cleaner (Boy Scout Rule).**
8. **Ambiguous → ask, don't guess.**

## How to adopt

**1. With Claude Code (global, auto-applied)** — copy the condensed layer into your user config so every session follows it:
```
~/.claude/CLAUDE.md        # condensed enforcement layer (auto-loaded)
~/.claude/CODING_RULES.md  # this repo's RULES.md, for deeper reference
```

**2. Per repository** — drop `RULES.md` at the repo root and link it from your project's `CLAUDE.md` / `CONTRIBUTING.md` so contributors and agents share one standard.

**3. Manually** — read [RULES.md](RULES.md) once, keep [checklists/PRE_COMMIT.md](checklists/PRE_COMMIT.md) next to your terminal.

## Versioning

This standard is versioned with [SemVer](https://semver.org/)-style intent:
- **MAJOR** — a rule is removed or reversed (may require existing code to change).
- **MINOR** — a new rule or section is added.
- **PATCH** — clarifications, examples, typo fixes.

See [CHANGELOG.md](CHANGELOG.md).

## Credits

The core standard is original. **Part 10 (Goal-Driven Execution)** and the surgical-change refinements (P0-5 / P0-7) were inspired by Andrej Karpathy's observations on common LLM coding mistakes, as packaged in [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

## License

[MIT](LICENSE) © Seongmin Noh ([@nohseongmin](https://github.com/nohseongmin))

Use it, fork it, adapt it to your team. Attribution appreciated, not required.
