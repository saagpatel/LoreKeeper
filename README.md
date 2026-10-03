# LoreKeeper

[![Rust](https://img.shields.io/badge/Rust-%23dea584?style=flat-square&logo=rust)](#) [![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](#) [![Platform](https://img.shields.io/badge/platform-macOS-lightgrey?style=flat-square)](#)

> Every command becomes a story — a text adventure where Rust keeps the world honest and a local LLM makes it feel alive.

LoreKeeper blends a traditional text adventure with a modern local-first stack. Rust manages typed world state and randomized combat, React and Tauri give it a real desktop feel, and an optional local LLM (via Ollama) turns your actions into living prose. If no model is available, the game runs on strong template narration — the experience never collapses into a broken demo.

```
You descend into the ruins of Thornhold, a fortress long abandoned
to darkness. Somewhere below, the Dungeon Heart pulses with ancient
power. Will you claim it, destroy it, or strike a deal with its keeper?
```

## Features

- **14 Handcrafted Locations + 5 Procedural Rooms** — A cohesive dark-fantasy world with secrets, hidden commands, and multiple endings
- **LLM-Powered Dialogue** — 7 handcrafted NPCs with persistent memory, relationships, and AI-generated responses when Ollama is available; graceful fallback to template narration when it isn't. The procedural dungeon adds 2 enemies
- **Built-In Map Editor** — Design custom adventures and export playable modules without touching code
- **Replay & Stats** — Command-log replay of completed games, run statistics, and a history browser to review past playthroughs
- **Theme Support** — Swap visual themes without restarting; custom color palettes for mood and accessibility
- **25+ Items + Crafting** — Discoverable items, a crafting system, and hidden combination recipes

## Quick Start

### Prerequisites

- Node.js 22 (22.12 or newer) and npm (CI uses Node 22); `package-lock.json` is canonical
- Rust 1.96.1 (pinned in `rust-toolchain.toml`) + Tauri v2 prerequisites for macOS
- [Ollama](https://ollama.ai) with a pulled model (optional — enhances NPC dialogue)

### Installation

```bash
git clone https://github.com/saagpatel/LoreKeeper.git
cd LoreKeeper
npm ci --ignore-scripts
cp .env.example .env
```

### Run (development)

```bash
npm run dev:lean
```

### Build (desktop app)

```bash
npm run tauri -- build
```

## Verification

Run commands from the repository root. `npm ci --ignore-scripts` installs the locked dependencies without running Husky's shared Git hook setup. `npm run dev` starts the browser frontend; `npm run dev:lean` starts the Tauri desktop development loop with temporary build caches. Desktop use can write local SQLite saves and contact the configured local Ollama service; use disposable test data for walkthroughs.

```bash
# Focused frontend fixture tests (no desktop or Ollama required)
npm run test:frontend -- src/lib/inputValidation.test.ts
# Frontend typecheck, all frontend tests, and production frontend build
npm run verify:frontend
# Rust Clippy (-D warnings) and Rust tests; requires the Tauri platform toolchain
npm run verify:full
```

There is no separate frontend lint/format script; `typecheck` and the commands above are defined in [package.json](package.json). Keep the [canonical local gate](.codex/verify.commands) and its runner `bash .codex/scripts/run_verify_commands.sh` for the required Git and performance checks. Focused tests do not replace that gate or required CI. Linux Rust verification needs the GTK/WebKit packages listed in [CI](.github/workflows/ci.yml); Rust wrappers default to `~/.cache/lorekeeper/cargo-target`, overridable with `CARGO_TARGET_DIR`.

For changed UI or user flows, install the Playwright Chromium prerequisite with `npx playwright install chromium`, then run `npm run test:e2e` (or append a spec path). The harness starts a fresh loopback Vite server and uses mocked Tauri IPC; it does not prove the packaged desktop app or real model behavior. Follow [the internal macOS release guide](docs/internal-release-macos.md) for broader release verification and disposable-data desktop checks. Pure documentation edits do not require a browser walkthrough.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Desktop shell | Tauri 2 + Rust |
| Game engine | Rust (world state, parser, crafting) |
| Frontend | React + TypeScript + Vite |
| AI narration | Ollama (local LLM, optional) |
| Storage | SQLite (save slots, replay data) |

## Architecture

The game engine lives entirely in Rust: the world state machine, command parser, NPC memory records, item system, and crafting rules are all managed as typed Rust structs with serializable state. Tauri exposes the engine via a typed command surface to the React frontend, which handles rendering the narrative output, the map panel, and the stats sidebar. Template responses are returned with each command, while Ollama dialogue is generated asynchronously and streamed as additional output. Narration emits a fallback event on errors or timeouts; the HTTP client has a 30-second total request timeout (including the stream), and stream consumption is separately capped at 10 seconds.

## License

MIT
