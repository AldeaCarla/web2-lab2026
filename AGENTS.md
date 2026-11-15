# AGENTS.md — web2-lab2026

<!-- graffino-agentic:begin v1 -->
## How we work here

**Before work:** fetch and fast-forward pull when a remote and upstream are configured.

**Always load** (indexes; open the entry before relying on it): `docs/agents/DECISIONS.md`, `docs/agents/PATTERNS.md`, `docs/agents/CONTRACTS.md`, `docs/agents/OPEN-QUESTIONS.md`, and `CONTEXT.md`.

**Load on demand:**

| When | Load |
|---|---|
| Structural work | `docs/agents/ARCHITECTURE.md` |
| Old code or a past decision needs explaining | `docs/agents/DECISIONS-SUPERSEDED.md`, `PATTERNS-SUPERSEDED.md` (never obeyed) |

**Flow:** use the `plan` skill for non-trivial changes or design choices. Build only after the user approves the plan, using the `build` skill. Small fixes can go straight to build, with tests first when tests exist.

### The gate
Ask: **does this change something already approved?**
- **Do, then report:** add a `candidate` pattern; add an open question; record a proposed answer supported by evidence; fix a typo, link, or moved path; follow approved entries.
- **Propose and wait:** change, supersede, or delete an approved entry; promote a candidate; add a decision; or contradict an entry. Check `docs/agents/REJECTED.md` first.

### Hard rules
- Build only from approved entries. Record unknown facts as open questions; never guess.
- Add nothing nobody asked for. Record suggestions instead.
- Make the smallest change that meets the approved criteria. Give a concrete reason for new abstractions, dependencies, options, or broad refactors.
- Write tests first where tests exist. Never weaken an assertion to make code pass.
- Never write secrets, tokens, passwords, or personal data into files, commits, or logs.
- Never push, tag, release, or deploy without the user's approval for that action.
- The code wins over documents. When they disagree, report and correct the document.
- Report failed and skipped checks with their reasons.

### Ledger rules
- IDs use one sequence per kind: `D-`, `PATTERN-`, `C-`, and `Q-`. Never renumber them.
- A provided contract lives here only. Consumers link to it. Changing one requires a proposal.
- Keep superseded entries; the lint tool moves them out of live indexes.
- After ledger changes, run `python3 docs/agents/tools/ledger-lint.py --fix`, then run it without `--fix`.
- Record reusable practices as candidate patterns. Propose settled constraints as decisions.
<!-- graffino-agentic:end -->

## 1. What this repo is
The repository contains the approved P25 plan for a web application that reconciles an association's bank statements, obligations, payments, penalties, and reports, with a member portal. The application code and runtime are not implemented yet; see Q-001 for runtime details.

## 2. Stack and commands
The planned stack is recorded in D-001. No project commands or installed versions are verified yet; see Q-002.

## 3. Layout
- `docs/agents/` — Graffino agent guidance and knowledge ledger.
- `.agents/skills/` — Codex project skills for planning and building.

## 4. Rules for this repo
- Follow the approved constraints in `docs/agents/DECISIONS.md`.
- Keep DEMO and REAL data strictly separated as specified in D-006.

## 5. Release
No release process is documented. See Q-003.

## 6. Definition of done
- Run the project's verified tests and build checks once they are documented.
- Keep the agent knowledge ledger lint clean.
- Record new practices as candidate patterns and unknowns as Q- entries.
- Keep DEMO and REAL data separated and preserve exact monetary and reconciliation calculations.
