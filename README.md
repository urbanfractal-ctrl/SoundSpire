# Spire

> *The music shifts before you finish the sentence.*

Spire is a real-time audio companion for tabletop RPG Game Masters. It runs silently on the laptop already at your table, listens to your narration, and automatically crossfades ambient music and triggers sound effects to match the scene — without you touching a thing.

**You’re narrating. Not operating software.**

-----

## What It Does

Spire listens to the GM’s voice during a session and uses a combination of keyword detection and vocal mood analysis to drive dynamic audio in real time.

- Detects scene context from what you say and how you say it
- Crossfades between mood-appropriate music automatically
- Triggers sound effects on keyword detection
- Brings your own music library — Spire makes it smart
- Runs entirely on-device. Your voice never leaves your machine.

Two operating modes are assigned automatically during setup:

- **Autonomous** — for expressive GMs whose delivery varies across moods. Spire drives transitions on its own; you correct with a hotkey when it gets it wrong.
- **Collaborative** — for measured, consistent narrators. Spire surfaces mood suggestions; you confirm with a single keypress. You’re always the director.

-----

## Status

Early development. Planning documentation is complete and locked. Implementation is underway.

|Phase            |Status                                             |
|-----------------|---------------------------------------------------|
|MVP Strategy     |✅ Locked — see `ttrpg_companion_mvp_strategy_v4.md`|
|Architecture Spec|✅ Locked — see `CLAUDE_v3.md`                      |
|Technical Spikes |🔄 In progress                                      |
|MVP Build        |⏳ Not started                                      |

-----

## Tech Stack

- **App framework:** Tauri 2.x (Rust + Svelte)
- **Speech-to-text:** whisper.cpp (medium model, local, no network)
- **Speaker verification:** Resemblyzer via ONNX Runtime
- **Vocal mood analysis:** emotion2vec+ base via ONNX Runtime
- **Audio engine:** FMOD
- **Database:** SQLite
- **Platform:** Windows + macOS desktop

All ML inference runs natively on-device via ONNX Runtime. No Python runtime. No cloud dependency.

-----

## Repository Structure

```
spire/
├── CLAUDE_v3.md                          ← Architecture spec and build reference
├── ttrpg_companion_mvp_strategy_v4.md    ← Product strategy and design decisions
├── src-tauri/                            ← Rust backend (Tauri)
├── src/                                  ← Svelte frontend
├── content/                              ← Keywords, passages, audio manifests
├── models/                               ← ONNX model files (gitignored, downloaded via script)
├── tools/                                ← Audio tagger CLI (Python, not shipped)
├── scripts/                              ← Model export and setup scripts
├── spikes/                               ← Throwaway exploration code
└── tests/                                ← Integration tests and fixtures
```

-----

## Planning Documents

Two documents drive this project and serve as the authoritative reference across all sessions and contributors:

**`ttrpg_companion_mvp_strategy_v4.md`** — The product bible. Covers concept, design philosophy, feature scope, mood taxonomy, detection architecture, voice training, monetisation, licensing, privacy/compliance, and go-to-market thinking.

**`CLAUDE_v3.md`** — The technical specification. Covers the full stack, module breakdown, acceptance criteria, data schemas, detection FSM, IPC contracts, coding conventions, performance budgets, and phase gates. This is the single source of truth for every architectural decision.

-----

## Core Design Principle

> **“The GM is narrating, not operating software.”**

Every feature is evaluated against this. If it requires the GM to stop talking to their players and think about the app, it’s wrong.

-----

## Privacy

Spire processes all audio locally. Voice data is never transmitted to any server. Vocal profiles are encrypted at rest using AES-256-GCM with OS-managed key storage (macOS Keychain / Windows DPAPI). The app is designed for full GDPR compliance with granular consent flows and a one-click profile deletion option.

-----

## Genre Support

**Launch:** Fantasy (English + Greek)

Planned: Horror, Sci-fi, Modern, Superheroic — sequenced by community demand.

Each genre ships as a content and intelligence bundle: curated audio, tuned keyword vocabulary, and genre-specific detection behaviour.

-----

## Bring Your Own Music

Spire is an intelligence layer, not a music library. Point it at your existing collection — it analyses and tags tracks automatically into mood categories, then makes your library session-aware. A bundled starter library (CC-licensed + commissioned originals) is included.

-----

## License

To be determined prior to public release.

-----

*Built by a solo founder and GM who got tired of pausing mid-sentence to change the music.*
