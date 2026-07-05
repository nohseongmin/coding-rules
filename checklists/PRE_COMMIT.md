# Pre-Commit Checklist

A 30-second gate before every commit. Full rationale in [../RULES.md](../RULES.md).

- [ ] **No hardcoding** — no secrets / URLs / paths / magic values in source. (Part 3)
- [ ] **Input validated** — type/length/format/range checked, allowlist first. (SEC-1)
- [ ] **No injection** — parameterized SQL, no `shell=True`, escaped HTML output. (SEC-2)
- [ ] **Errors handled** — nothing swallowed; resources cleaned up; logger not `print`. (Part 5)
- [ ] **One job per function** — small, intent-revealing names, ≤3 args. (Parts 1–2)
- [ ] **Consistent** — matches surrounding naming/structure/style. (P0-2)
- [ ] **Clean** — no dead code, debug prints, or commented-out blocks.
- [ ] **Understood** — I can explain *why* this works. (P0-1)
- [ ] **In scope** — no drive-by rewrites outside the request. (P0-5)
- [ ] **Tested** — edge and failure paths, not just the happy path. (Part 7)
- [ ] **Verifiable goal** — success criterion defined and checked (test/build/run), not "looks right." (Part 10)

**Definition of Done:** working ≠ done. Done = works + all boxes above + tested.
