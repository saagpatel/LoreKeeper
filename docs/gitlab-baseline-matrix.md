# GitLab Baseline Matrix

Choose the smallest baseline that matches the repo.

## Library baseline

- Use when:
  - the repo is primarily a package, crate, SDK, or shared module
- Required jobs:
  - one verify job for tests, linting, and build validation
  - `secret_detection`
- Optional jobs:
  - `dependency_scanning` when GitLab features support it
- Playwright:
  - usually omit
- Rust pinning:
  - required only for Rust libraries
- Repo-specific notes:
  - keep the CI fast and minimal
  - avoid browser or desktop baggage

## Web app baseline

- Use when:
  - the repo ships a browser-first product or serious frontend
- Required jobs:
  - one verify job for typecheck, tests, and production build
  - `secret_detection`
- Optional jobs:
  - `dependency_scanning` when GitLab features support it
  - `e2e_tests` when the app has meaningful browser flows worth guarding
- Playwright:
  - include only when there are real user flows, not just trivial smoke checks
- Rust pinning:
  - omit unless the repo actually includes Rust
- Repo-specific notes:
  - artifact paths and retention should be explicit
  - keep runtime versions pinned if the build tooling is sensitive

## Desktop app baseline

- Use when:
  - the repo combines frontend code with Rust or native desktop packaging
- Required jobs:
  - one verify job that covers both frontend and Rust correctness
  - `secret_detection`
- Optional jobs:
  - `dependency_scanning` when GitLab features support it
  - `e2e_tests` when browser-level or app-shell flows are meaningful
- Playwright:
  - include when it protects real product behavior or release confidence
- Rust pinning:
  - required
- Repo-specific notes:
  - expect extra Linux packages or a custom CI image
  - prefer a custom CI image once the baseline is reused across multiple repos

## Dependency scanning expectations

- Baseline-desired across repo types
- Conditionally reusable, not guaranteed
- Expect it only when the project plan and GitLab features actually produce the job
- Treat the absence of a materialized dependency-scanning job as a GitLab capability issue before assuming the repo YAML is wrong
