# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-03-22

### Added
- Initial commit: LoreKeeper text adventure game
- 16 features across 6 phases: sound, difficulty, achievements, NPC memory, status effects, journal, crafting, secrets, LLM dialogue, replay, dungeon, map editor
- Complete Phase 1 integration testing
- Complete Phase 2 content expansion: quests, items, NPCs, and achievements
- Complete Phases 3-5: UI/UX, performance, and extensibility
- Lean dev mode and cleanup commands
- Lockfile for deterministic installs
- GitLab pipeline with verify_full and e2e tests

### Fixed
- Bugs, security issue, and performance problems found in codebase audit
- Audit round 2 findings: security, accessibility, and robustness
- Inline format args for clippy
- Match e2e image to Playwright lockfile
- Align e2e container with Playwright runtime
- Align Rust toolchain with GitLab verify job
- Align GitLab security jobs with pipeline stages
- Skip branch guard on default-branch pushes

### Changed
- Bootstrap codex tests and docs defaults
- Prune unused files and tighten lint hygiene
- Codify and harden GitLab baseline policy
- Stabilize Playwright versioning and reports
- Refresh project overview and add screenshots to README
- Extract reusable GitLab baseline guidance
