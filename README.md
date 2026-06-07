# LoreKeeper

[![Rust](https://img.shields.io/badge/Rust-%23dea584?style=flat-square&logo=rust)](#) [![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](#) [![Platform](https://img.shields.io/badge/platform-macOS-lightgrey?style=flat-square)](#)

> Every command becomes a story — a text adventure where Rust keeps the world honest and a local LLM makes it feel alive.

LoreKeeper blends a traditional text adventure with a modern local-first stack. Rust manages world state with deterministic correctness, React and Tauri give it a real desktop feel, and an optional local LLM (via Ollama) turns your actions into living prose. If no model is available, the game runs on strong template narration — the experience never collapses into a broken demo.

```
You descend into the ruins of Thornhold, a fortress long abandoned
to darkness. Somewhere below, the Dungeon Heart pulses with ancient
power. Will you claim it, destroy it, or strike a deal with its keeper?
```

## Features

- **14 Handcrafted Locations + 5 Procedural Rooms** — A cohesive dark-fantasy world with secrets, hidden commands, and multiple endings
- **LLM-Powered Dialogue** — 7 NPCs with persistent memory, relationships, and AI-generated responses when Ollama is available; graceful fallback to template narration when it isn't
- **Built-In Map Editor** — Design custom adventures and export playable modules without touching code
- **Replay & Stats** — Full session replay, run statistics, and a history browser to review past playthroughs
- **Theme Support** — Swap visual themes without restarting; custom color palettes for mood and accessibility
- **25+ Items + Crafting** — Discoverable items, a crafting system, and hidden combination recipes

## Quick Start

### Prerequisites

- Node.js 20+
- pnpm 9+
- Rust toolchain (stable) + Tauri v2 prerequisites for macOS
- [Ollama](https://ollama.ai) with a pulled model (optional — enhances NPC dialogue)

### Installation

```bash
git clone https://github.com/saagpatel/LoreKeeper.git
cd LoreKeeper
pnpm install
cp .env.example .env
```

### Run (development)

```bash
pnpm dev
```

### Build (desktop app)

```bash
pnpm build
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Desktop shell | Tauri 2 + Rust |
| Game engine | Rust (world state, parser, crafting) |
| Frontend | React + TypeScript + Vite |
| AI narration | Ollama (local LLM, optional) |
| Storage | SQLite (save slots, replay data) |

## Architecture

The game engine lives entirely in Rust: the world state machine, command parser, NPC memory graph, item system, and crafting rules are all managed as typed Rust structs with serializable state. Tauri exposes the engine via a typed command surface to the React frontend, which handles rendering the narrative output, the map panel, and the stats sidebar. Ollama dialogue is generated asynchronously and streamed into the NPC response — if the model is slow or unavailable, the template fallback fires immediately so the player is never blocked.

## License

MIT
