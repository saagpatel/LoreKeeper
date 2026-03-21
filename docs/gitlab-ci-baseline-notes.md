# GitLab CI Baseline Notes

## Purpose

This repo is the reference baseline for serious solo-builder GitLab projects that want:

- pinned, boring CI
- merge-request-first workflows
- automatic merge after healthy pipelines
- secret detection by default
- Playwright e2e coverage when it adds real value

## Baseline classes

Use one of these baseline classes instead of forcing one CI file onto every repo:

- Tauri/Rust desktop baseline
  - `verify_full`
  - `secret_detection`
  - optional `dependency_scanning`
  - optional `e2e_tests`
- Frontend/web baseline
  - frontend verify job
  - `secret_detection`
  - optional `dependency_scanning`
  - optional `e2e_tests`
- Static/lightweight baseline
  - minimal verify job
  - `secret_detection`

## Default CI policy

- Run pipelines only for:
  - merge requests
  - the default branch
- Keep merge commits as the default history shape.
- Auto-merge only after the merge request is healthy and discussions are resolved.
- Keep source branches after merge unless there is an explicit cleanup decision.

## Version pinning policy

- Pin Rust toolchains exactly.
- Pin Playwright exactly and match the Docker image version to the resolved package version.
- Prefer deterministic versions for CI-critical tools over floating semver ranges.

## Security policy

- `secret_detection` is part of the default baseline.
- Keep `secret_detection` blocking at the job level.
- Blocking at the job level means scanner execution failures block merges. Findings themselves still flow through GitLab security reports unless you add a separate finding-gate policy.
- GitLab dependency scanning is Ultimate-tier. Include it only when you have confirmed the project plan/features actually support it.
- Treat dependency scanning as advisory first, then decide later if it should become blocking.

## Caching and artifacts

- Cache package-manager artifacts like `.npm/`, not `node_modules/`, across mixed CI images.
- Default Playwright artifact retention:
  - `playwright-report/`
  - `test-results/`
  - `expire_in: 30 days`

## GitHub coexistence policy

- GitHub workflows may remain as legacy/fallback automation.
- GitLab CI is the canonical merge gate while GitLab is the active remote workflow.

## Fast-follow optimization

- The next performance upgrade for Tauri/Rust repos is a custom CI image with:
  - Rust
  - Node
  - GTK / WebKit build dependencies
- That should reduce repeated package installation and cut several minutes from `verify_full`.
