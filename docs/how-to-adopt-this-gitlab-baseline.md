# How To Adopt This GitLab Baseline

Use LoreKeeper as a reference repo, not as a bundle to copy wholesale.

## Start here

1. Pick the smallest baseline class from [`docs/gitlab-baseline-matrix.md`](docs/gitlab-baseline-matrix.md).
2. Copy only the relevant CI structure into the new repo.
3. Configure the GitLab UI settings from [`docs/gitlab-new-repo-checklist.md`](docs/gitlab-new-repo-checklist.md).
4. Open a merge request and confirm the expected jobs actually appear.

## What to copy

- `workflow: rules` that limit pipelines to merge requests and the default branch
- the intentional stage layout:
  - `verify`
  - `test`
  - `e2e`
- the `secret_detection` baseline policy
- artifact retention defaults when Playwright is present
- version pinning policy for CI-critical tools
- the README language that makes GitLab the canonical merge gate

## What to customize

- the main verify job name and command
- runtime images
- package-manager commands
- whether `e2e_tests` exists at all
- whether Rust pinning exists at all
- dependency scanning posture if GitLab actually materializes the job

## What to delete if not needed

- `e2e_tests` for repos without meaningful browser flows
- Playwright artifact paths for repos without Playwright
- Rust toolchain pinning for non-Rust repos
- desktop Linux package installs for non-desktop repos

## What must be configured in GitLab UI

- default-branch protection
- force-push policy
- pipeline-required merge gate
- discussion-resolution requirement
- merge strategy
- squash policy
- source-branch retention choice

These settings are part of the baseline, but they are not stored in repo files.

## What not to cargo-cult from LoreKeeper

- Tauri and GTK/WebKit package installs
- desktop-app-specific verification commands
- game-specific scripts and internal release docs
- Playwright job presence unless the repo truly benefits from it
- Rust toolchain pinning unless the repo actually uses Rust

## Asset buckets

### Reusable baseline assets

- GitLab CI policy language
- merge-gate guidance
- artifact retention defaults
- version pinning guidance
- source-branch retention guidance
- GitHub-versus-GitLab canonical gate language

### Repo-type-specific assets

- desktop verify job shape
- Playwright e2e job
- Rust toolchain pinning
- custom CI image recommendation for heavy stacks

### LoreKeeper-only assets

- game-specific docs and screenshots
- internal release docs
- LoreKeeper verification command names where they reflect project-specific scripts

## Rule of thumb

If a new repo does not clearly need a job, runtime, or package install, leave it out and add it later only when it earns its keep.
