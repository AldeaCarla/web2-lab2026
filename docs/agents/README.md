# docs/agents — how this repo works with AI agents

Codex reads `AGENTS.md` for repository guidance. Project skills live in `.agents/skills/`.

| Need | File |
|---|---|
| Project vocabulary | `../../CONTEXT.md` |
| Decisions and patterns | `DECISIONS.md`, `PATTERNS.md` |
| Interfaces | `CONTRACTS.md` |
| Structure | `ARCHITECTURE.md` |
| Unknowns | `OPEN-QUESTIONS.md` |
| Explain old choices | `DECISIONS-SUPERSEDED.md`, `PATTERNS-SUPERSEDED.md` |
| Check or rebuild ledger indexes | `python3 docs/agents/tools/ledger-lint.py` or add `--fix` |

Use the `plan` skill for non-trivial changes and the `build` skill after plan approval.
