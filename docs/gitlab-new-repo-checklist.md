# GitLab New Repo Checklist

Use this checklist when bringing a new repo onto the GitLab baseline.

## 1. Pick the smallest baseline that fits

- Choose one baseline class from [`docs/gitlab-baseline-matrix.md`](docs/gitlab-baseline-matrix.md).
- Do not start from the desktop baseline unless the repo actually needs Rust, desktop packaging, or Playwright browser flows.

## 2. In-repo setup

- Add `.gitlab-ci.yml` using the chosen baseline class.
- Pin CI-critical versions that the repo depends on.
- Add any required repo-root artifact paths to `.gitignore`.
- Add a short README note that GitLab CI is the canonical merge gate.
- Add or adapt any repo-specific verification command such as `verify_full`, `verify_frontend`, or library test commands.

## 3. GitLab UI settings

These are manual and must be confirmed in GitLab itself:

- Protect the default branch.
- Disable force-push on the default branch.
- Require green pipelines before merge.
- Require resolved discussions before merge.
- Choose merge strategy:
  - default recommendation: merge commits
- Choose squash policy:
  - default recommendation: off unless the repo has a strong reason to squash
- Decide whether source branches are kept after merge:
  - default recommendation: keep unless there is an explicit cleanup policy
- Confirm GitLab CI is the canonical merge gate for this repo.

## 4. Security defaults

- Keep `secret_detection` enabled by default.
- Make `secret_detection` blocking at the job level.
- Include dependency scanning only if GitLab features for the project actually materialize it as a runnable job.
- If dependency scanning is available, start advisory first unless the signal quality is already known to be good.

## 5. Validation before first merge

- Lint the CI config.
- Open a merge request against the default branch.
- Confirm the expected jobs actually appear in the pipeline.
- Confirm artifact upload paths are correct.
- Confirm the merge gate behaves the way the project policy says it should.

## 6. Final sanity check

- Make sure the baseline is not carrying LoreKeeper-only baggage such as desktop-only package installs, internal release docs, or game-specific verification assumptions.
- Make sure GitHub workflows, if retained, are documented as secondary unless the repo explicitly says otherwise.
