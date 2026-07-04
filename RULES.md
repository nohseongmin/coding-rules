# Coding Standards — Full Reference (v1.0.0)

> A language-agnostic engineering standard. Goal: stop the 3 pains of AI/vibe-coding — **hardcoding, inconsistency, security holes** — with rules you can actually follow.
>
> **How to read:** each rule is **RULE** (imperative) → **WHY** → optional **BAD / GOOD**. Applies across languages. When a case is ambiguous, Part 0 (Prime Directives) wins.

---

## Part 0 — Prime Directives (highest priority)

- **P0-1 Understand before you write.** If you can't explain *why* pasted or generated code works, don't commit it — AI output included.
- **P0-2 Match the codebase.** New code follows the surrounding naming, structure, patterns, and error-handling style. Consistency > personal taste.
- **P0-3 No silent hardcoding.** Before baking a literal in, ask: "does this vary by env / user / secret / time?" If yes → constant, config, or argument. (→ Part 3)
- **P0-4 Never invent secrets or endpoints.** No API keys, URLs, account IDs, or paths in source. Use env/config placeholders. (→ Part 3)
- **P0-5 Make the smallest change that works.** Don't refactor or rewrite outside the request's scope — propose it separately.
- **P0-6 Fail loudly, don't swallow errors.** Log and handle; if unrecoverable, fail clearly. (→ Part 5)
- **P0-7 Leave it cleaner (Boy Scout Rule).** Fix the small smells you touch; never make a file worse. (→ Part 6)
- **P0-8 When ambiguous, ask — don't guess.** Wrong assumptions are the #1 source of tech debt.

---

## Part 1 — Design Principles

### SOLID

- **S — Single Responsibility.** A module/class/function should have one reason to change. Two reasons → split it. *Why:* one file doing UI + validation + DB + network breaks everywhere when any one changes.
- **O — Open/Closed.** Open to extension, closed to modification. Add new cases via polymorphism/strategy instead of carving up existing code with more `if`s.
- **L — Liskov Substitution.** A subtype must be usable wherever its base type is, without breaking the contract. An override that throws or changes meaning means the inheritance is wrong.
- **I — Interface Segregation.** Prefer several small, purpose-built interfaces over one fat interface. Don't force clients to depend on methods they don't use.
- **D — Dependency Inversion.** High-level policy shouldn't depend on low-level details. Both depend on abstractions — makes testing and swapping easy.

```text
BAD:  class ReportService { new MySqlDb().save(...) }    // bound to a concrete DB
GOOD: class ReportService(db: DataStore) { db.save(...) } // depends on an abstraction, injected
```

### The core three

- **DRY (Don't Repeat Yourself).** Don't duplicate knowledge — **but extract on the 3rd duplication (Rule of Three)**. Premature abstraction is worse than duplication.
- **KISS (Keep It Simple).** Ship the simplest thing that works. Five readable lines beat one clever line.
- **YAGNI (You Aren't Gonna Need It).** Don't build features/abstractions/config "in case." Only what's needed now.

### Coupling, cohesion, boundaries

- **High Cohesion / Low Coupling.** Things inside a module belong together; dependencies between modules stay thin.
- **Separation of Concerns.** Don't mix UI / business logic / data access. Keep layers apart.
- **Law of Demeter.** No `a.getB().getC().doThing()` — talk only to immediate collaborators. Chains couple you to internal structure.
- **Composition over Inheritance.** Inherit only for a true "is-a." Most of the time composition (has-a) is more flexible.
- **Principle of Least Astonishment.** A function/API does exactly what its name promises — no hidden side effects.
- **Fail Fast.** Reject invalid input/state immediately, close to the source. The later it blows up, the harder to trace.

---

## Part 2 — Clean Code

### Naming
- Intent-revealing names. No `d`, `tmp`, `data2`. `daysSinceLastTrade` reads as its meaning.
- Booleans as `is/has/should`. Functions are verbs, variables are nouns.
- Searchable names; single letters only for tiny loop indices.
- Same concept → same word project-wide (don't mix `user` / `account` / `member`).

### Functions
- **Small.** One job per function; ideally fits on screen (~≤30 lines as a guide).
- **Few args.** 0–2 ideal; beyond 3, pass an object.
- **No boolean flag args.** `render(true)` is meaningless — split into two functions.
- **No hidden side effects.** If the name promises a computation, don't mutate state.
- **One level of abstraction.** Don't mix high-level flow and low-level bit-twiddling in one function.

### Control flow — guard clauses / early return
```text
BAD:                              GOOD:
if (user) {                       if (!user) return DENY;
  if (user.active) {              if (!user.active) return DENY;
    if (hasPermission) {          if (!hasPermission) return DENY;
      doWork();                   doWork();
    }
  }
}
```
Nesting past 3 levels is a refactor signal.

### Comments
- Explain **why**, not **what**. Code says what it does; comments capture the *reason*, the trade-off, the gotcha.
- Delete dead / commented-out code (git remembers).
- `TODO`s carry context and a condition — never a bare `// TODO fix`.

### No magic values
```text
BAD:  if (retries > 3) ...            price * 0.0025
GOOD: MAX_RETRIES = 3                 TRADING_FEE_RATE = 0.0025
      if (retries > MAX_RETRIES)      price * TRADING_FEE_RATE
```
Obvious values (0/1/"") and very local literals are fine.

---

## Part 3 — No Hardcoding & Configuration

Principle: **code is logic, values are configuration.** "Does this vary by deploy env / user / secret / time?" → if yes, get it out of the code.

- **C-1 Secrets → env / secret manager.** API keys, tokens, passwords, account IDs **never** in source. Use `.env` (gitignored) or a secret manager; code reads `os.environ["X"]` only.
- **C-2 Environment-specific → config.** URLs, ports, paths, timeouts, feature toggles come from config/env, not literals.
- **C-3 Business constants → one named-constants module.** Fees, thresholds, retry counts, periods live in one place with names.
- **C-4 Twelve-Factor config.** Config lives in the environment; the same build runs in dev/prod with only config changed.
- **C-5 Fail on missing config.** If a required setting is absent, die clearly at startup — not later with a mysterious `None`.
- **C-6 No secrets in logs / errors / git history.** If one leaks into git, **rotate** it and scrub history. Gitignore `.env`, `*.key`, `secrets/` first.

```text
BAD:  headers = {"Authorization": "Bearer sk-live-abc123..."}
      db = connect("prod-db.internal:5432")
GOOD: headers = {"Authorization": f"Bearer {os.environ['API_TOKEN']}"}
      db = connect(settings.DB_URL)   # from env, validated at startup
```

---

## Part 4 — Security (OWASP-minded)

Focused on where fast-generated code gets breached.

- **SEC-1 Never trust input.** Everything from users / network / files / env is outside the trust boundary. Validate type/length/format/range (allowlist first) before use.
- **SEC-2 No injection.** Never build queries/commands/HTML by string concatenation.
  - SQL → **parameterized queries** (`?` / named). No f-string SQL.
  - Shell → pass an argument array; no `shell=True` / string commands.
  - HTML → encode/escape output (XSS). Never put user data into `innerHTML`.
- **SEC-3 Secrets management.** See Part 3 C-1. A hardcoded key is an immediate incident.
- **SEC-4 AuthN/AuthZ + least privilege.** Separate authentication from authorization; check *permission* on every sensitive action. Tokens/keys/DB accounts get the minimum privilege.
- **SEC-5 Secure defaults.** Default to the safest setting. Make risky features opt-in; never make security opt-out.
- **SEC-6 Don't roll your own crypto.** Use standard libraries. Passwords → slow salted hash (bcrypt/argon2), never MD5/SHA1. Randomness → CSPRNG (`secrets`).
- **SEC-7 Dependencies / supply chain.** Trusted packages only, pinned versions (lockfile), regular vuln scans (`pip-audit` / `npm audit`). Watch for typosquatting.
- **SEC-8 Don't leak in errors/logs.** No stack traces, internal paths, secrets, or PII to users. Mask secrets/PII in logs too.
- **SEC-9 Transport & storage.** TLS for external comms; encrypt sensitive data at rest; never disable cert verification (`verify=False`).
- **SEC-10 File/path safety.** Block traversal (`../`) when building paths from input. Validate upload type/size; store outside executable paths.
- **SEC-11 Rate limit & resource caps.** Throttle public endpoints; prevent unbounded loops/memory allocation.

```text
BAD:  cur.execute(f"SELECT * FROM users WHERE id = {uid}")     # SQLi
      os.system("ping " + host)                                 # command injection
      el.innerHTML = userComment                                # XSS
GOOD: cur.execute("SELECT * FROM users WHERE id = ?", (uid,))
      subprocess.run(["ping", host])                            # no shell
      el.textContent = userComment                              # escaped
```

---

## Part 5 — Error Handling & Logging

- **E-1 Don't swallow.** No empty `except:` / `catch {}`. Handle, rethrow, or log.
- **E-2 Catch narrow.** Catch the exceptions you expect, not a blanket `Exception`.
- **E-3 Fail fast, degrade gracefully.** Unrecoverable → fail fast. Partial failure → a meaningful user message that hides internals.
- **E-4 No control flow via exceptions.** Don't route the normal path through `try/except`.
- **E-5 Clean up resources.** Files/sockets/connections released via `with` / `try-finally` / `defer`.
- **E-6 Structured, leveled logging.** Use a logger, not leftover `print`s. Respect levels (debug/info/warn/error). Never log secrets/PII (SEC-8).

---

## Part 6 — Refactoring & Code Smells

- **R-1 Refactor with a safety net.** Refactor when tests exist; if not, add characterization tests first.
- **R-2 Small, reversible steps.** One thing at a time. Don't mix behavior changes and structure changes in one commit.
- **R-3 Rule of Three.** Extract on the 3rd duplication.
- **R-4 Boy Scout Rule.** Fix one small smell as you pass through.

**Common smells → fix:**
- God function/class (too many jobs) → split (SRP)
- Long parameter list → parameter object
- Deep nesting → guard clauses
- Duplicated code → extract (after Rule of Three)
- Magic numbers → named constants
- Feature envy (a function that only touches another object's data) → move it to the data
- Shotgun surgery (one change needs edits in many places) → redesign for cohesion
- Dead / commented-out code → delete
- Primitive obsession (domain modeled as raw strings/ints) → small value objects

---

## Part 7 — Testing

- **T-1 Test behavior, not implementation.** Verify the public contract; tests bound to internals block refactoring.
- **T-2 AAA.** Arrange → Act → Assert. One thing per test.
- **T-3 Test the edges.** Empty, boundary, null, negative, large, and failure paths — not just the happy path.
- **T-4 Deterministic.** Inject/mock time, randomness, and network. Fix or quarantine flaky tests immediately.
- **T-5 Fast feedback.** Many fast unit tests, few integration/E2E (test pyramid).
- **T-6 Tests are code.** Clean-code rules apply to tests too.

---

## Part 8 — Architecture & Project Structure

- **A-1 Layers with one-way dependencies.** UI → domain/services → data. The inner (domain) must not know the outer (framework/DB) — DIP.
- **A-2 Consistent structure.** Predictable folder conventions per project; same kind of thing in the same place.
- **A-3 Explicit boundaries.** Modules talk through defined interfaces; don't let a neighbor reach into internals.
- **A-4 Config at the edge.** Load/validate config at the entry point; keep core logic pure.
- **A-5 Document the "why".** README covers how to run, structure, and key decisions. Record architecture decisions briefly (ADRs).

---

## Part 9 — Version Control & Collaboration

- **V-1 Small, focused commits.** One logical change per commit; the message explains *why* (imperative mood).
- **V-2 Never commit secrets/artifacts.** `.env`, keys, build outputs, large binaries stay gitignored. If leaked, rotate immediately.
- **V-3 Branch off main; don't push unasked.** Don't commit/push straight to the default branch; push only when asked.
- **V-4 Review before merge.** Read the diff before merging, even solo. Review generated code especially against this standard.

---

## Appendix — Pre-Commit Checklist

- [ ] No hardcoded secrets / URLs / paths / magic values? (Part 3)
- [ ] Input validated + injection defended? (SEC-1, SEC-2)
- [ ] Errors handled/logged, not swallowed? (Part 5)
- [ ] Each function does one thing? Names reveal intent? (Parts 1–2)
- [ ] Consistent with surrounding code style? (P0-2)
- [ ] Dead code / debug prints / commented-out code removed?
- [ ] Can I explain *why* I made this change? (P0-1)
- [ ] No changes outside the requested scope? (P0-5)

## Definition of Done

Working ≠ done. **Done = works + checklist passed + tested + consistent with the codebase + no hardcoded secrets/magic values.**
