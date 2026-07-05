# Changelog

All notable changes to this standard are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); the standard follows [SemVer](https://semver.org/)-style intent (see [CONTRIBUTING.md](CONTRIBUTING.md)).

## [Unreleased]

## [1.1.0] — 2026-07-05

### Added
- **Part 10 — Goal-Driven Execution**: turn vague tasks into verifiable goals ("make it work" → a check that must pass) and loop until they pass; state a one-line plan with a verify step for multi-step work.
- Pre-commit checklist item for a defined, verifiable success criterion.

### Changed
- Sharpened **P0-5** (smallest change) with an explicit orphan-cleanup rule: remove only the now-unused code your own change creates; leave pre-existing dead code but mention it.
- Scoped **P0-7** (Boy Scout Rule) to in-scope smells; surface unrelated smells instead of silently sweeping them into the diff.

### Credits
- Part 10 and the P0-5/P0-7 refinements draw on Andrej Karpathy's observations on common LLM coding mistakes ([multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)).

## [1.0.0] — 2026-07-05

### Added
- Initial release of the coding standard ([RULES.md](RULES.md)):
  - Part 0 — 8 Prime Directives (vibe-coding guardrails)
  - Part 1 — Design Principles (SOLID, DRY/KISS/YAGNI, coupling/cohesion, Law of Demeter, composition over inheritance, fail fast)
  - Part 2 — Clean Code (naming, functions, guard clauses, comments, no magic values)
  - Part 3 — No Hardcoding & Configuration
  - Part 4 — Security (OWASP-minded, SEC-1..SEC-11)
  - Part 5 — Error Handling & Logging
  - Part 6 — Refactoring & Code Smells
  - Part 7 — Testing
  - Part 8 — Architecture & Project Structure
  - Part 9 — Version Control & Collaboration
  - Appendix — Pre-Commit Checklist & Definition of Done
- [checklists/PRE_COMMIT.md](checklists/PRE_COMMIT.md) — extractable 30-second gate.
- [.editorconfig](.editorconfig) — cross-editor formatting consistency.
- MIT [LICENSE](LICENSE).

[Unreleased]: https://github.com/nohseongmin/coding-rules/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/nohseongmin/coding-rules/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/nohseongmin/coding-rules/releases/tag/v1.0.0
