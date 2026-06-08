<!-- comm-contract:start -->

## Communication Contract

- Inherit global Codex communication and reporting rules from `/Users/d/.codex/AGENTS.override.md` and `/Users/d/.codex/policies/communication/BigPictureReportingV1.md`.
- Repo-specific instructions below add project constraints only; do not restate global voice or status-reporting rules here.
<!-- comm-contract:end -->

## Inherited Operating Rules

- Inherit global git, review/fix, testing, docs, skill-use, and reporting gates from `/Users/d/.codex/AGENTS.md` and active session instructions.
- Use `.codex/verify.commands` and `.codex/scripts/run_verify_commands.sh` as this repo's local verification authority when present.
- Keep package-manager, maturity, automation-safety, and portfolio constraints below as repo-local overrides.

## Package Manager Contract
- `npm` is the canonical package manager for this repository.
- `package-lock.json` is the canonical lockfile.
- Do not introduce `pnpm`/`yarn` workflow paths unless a deliberate migration plan is approved.

## Feature Maturity Policy
- Default to `Stable` Codex features for production-critical paths.
- `Beta` and `Experimental` features require:
  - explicit owner,
  - documented fallback path,
  - and rollback note in release documentation.
- Multi-agent workflows are optional accelerators, not required release gates.

## Automation Safety Baseline
- Automations must be tested manually in a regular thread before scheduling.
- Prefer read-only or workspace-write sandbox modes by default.
- Any full-access automation requires explicit risk acknowledgement and command allowlisting rules.

<!-- portfolio-context:start -->
# Portfolio Context

## What This Project Is

LoreKeeper is an active local project in the /Users/d/Projects portfolio.

## Current State

Portfolio truth currently marks this project as `recent` with `boilerplate` context. Phase 104 recovered minimum-viable context so future sessions can resume without rediscovery.

## Stack

| Layer       | Tech                                                             |
| ----------- | ---------------------------------------------------------------- |
| Desktop app | [Tauri 2](https://tauri.app/)                                    |
| Game engine | Rust                                                             |
| Frontend    | React 19 + TypeScript (strict)                                   |
| Narration   | [Ollama](https://ollama.com/) (local LLM) with template fallback |
| Persistence | SQLite (saves, stats, achievements, themes)                      |

## How To Run

If you want to try the game locally as a developer:

```bash
npm ci
npm run dev:lean
```

## Known Risks

- This repo only has minimum-viable recovery context today; deeper handoff details may still live in the README and supporting docs.

## Next Recommended Move

Use this context plus the README and supporting docs to resume the next active task, then promote the repo beyond minimum-viable by capturing a dedicated handoff, roadmap, or discovery artifact.

<!-- portfolio-context:end -->
