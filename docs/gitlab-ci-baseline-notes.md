# GitLab CI Baseline Notes

## Purpose

Use LoreKeeper as a reference repo for serious solo-builder GitLab projects that want:

- deterministic, boring CI
- merge-request-first workflows
- GitLab as the canonical merge gate
- automatic merge only after healthy MR state
- security scanning by default without unnecessary ceremony

LoreKeeper is the current desktop-app reference, not a universal template. Future repos should copy the smallest baseline that fits.

## Required defaults

These defaults should apply to most GitLab repos unless there is a clear reason not to:

- Run pipelines only for merge requests and the default branch.
- Keep merge commits as the default history shape.
- Auto-merge only after the MR is healthy and discussions are resolved.
- Keep source branches after merge unless there is an explicit cleanup decision.
- Treat GitLab CI as the canonical merge gate.
- Keep `secret_detection` in the baseline and make it blocking at the job level.
- Cache package-manager artifacts such as `.npm/`, not `node_modules/`, across mixed CI images.
- Pin CI-critical tool versions instead of relying on floating versions.

## Optional defaults

These defaults are useful, but only when the repo type actually needs them:

- `e2e_tests` for repos with meaningful browser flows.
- Playwright artifacts at `playwright-report/` and `test-results/` with `expire_in: 30 days`.
- Rust toolchain pinning for Rust-based repos.
- A custom CI image for heavy stacks that repeatedly install the same runtime and system packages.

## Security policy

- `secret_detection` is a default baseline job.
- Keep `secret_detection` blocking at the job level.
- Blocking at the job level means scanner execution failures block merges. Findings still surface through GitLab security reports unless you add a separate finding gate.
- Dependency scanning is baseline-desired, but it is conditional.
- Include dependency scanning only when the project plan and GitLab features actually materialize it as a runnable job.
- If dependency scanning appears, start with advisory posture first and only make it blocking after the signal quality is acceptable.

## Version pinning policy

- Pin Rust toolchains exactly.
- Pin Playwright exactly and match the Docker image version to the resolved Playwright package version.
- Prefer deterministic versions for CI-critical tools over broad semver ranges.

## GitHub coexistence policy

- GitHub workflows may remain as legacy or fallback automation.
- GitLab CI is the canonical merge gate while GitLab is the active remote workflow.
- Future repos should state this explicitly in README or baseline docs so the merge path is unambiguous.

## Manual GitLab-side settings

These settings must be configured in GitLab and should not be assumed from repo files alone:

- protect the default branch
- disable force-push on the default branch
- require a green pipeline before merge
- require resolved discussions before merge
- choose merge strategy
- choose squash policy
- choose whether source branches are kept or removed after merge

Use [`docs/gitlab-new-repo-checklist.md`](docs/gitlab-new-repo-checklist.md) for the click-path checklist and [`docs/gitlab-baseline-matrix.md`](docs/gitlab-baseline-matrix.md) for repo-class selection.

## Fast-follow optimization

The next performance upgrade for the desktop baseline is a custom CI image with:

- Rust
- Node
- GTK and WebKit build dependencies

That should remove repeated package installation and cut several minutes from `verify_full`.
