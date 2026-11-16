# CopyCat Builder

A multi-agent, multi-project workspace and skill suite that turns YouTube project videos into verified, hardened software replicas through a strict **6-stage reverse-engineering and parity-auditing pipeline**.

Generated replicas live in isolated `projects/<project-slug>/` folders (gitignored by default so this repository stays a clean, reusable skill and template hub).

---

## Pipeline Overview (`0 -> 1 -> 2A + 2 -> 3 -> 4 -> 5`)

1. **Stage 0 — Preflight & Multimodal Signal Extraction**: Checks dependencies (`yt-dlp`, `youtube-transcript-api`, `ffmpeg`), extracts metadata, chapters, description links, timestamped transcripts, and scene/montage/chapter keyframes (`frame_MM_SS.png`) while filtering intro/sponsor noise.
   - *Multi-Project Triage (`Stage 0.5`)*: If a video contains multiple projects or ideas, CopyCat Builder pauses to let you pick which one to build first and creates resumable `projects/<project-slug>/PROJECT_PLACEHOLDER.md` stubs for the rest.
2. **Stage 1 — Replicability & Feasibility Gate**: Classifies the archetype (*Web/Full-Stack UI*, *CLI/Backend/AI*, or *Games/Creative Coding*), audits Hard vs. Soft blockers, builds a Bill of Materials (BOM) & Tool Substitutions table, and assigns a Tier 1–4 verdict (`Tier 4` halts immediately).
3. **Stage 2A + 2 — External Research & Reverse Specification**: Researches official docs, companion repos, and reference architectures (`[RESEARCHED: <URL>]`) to resolve `[UNSPECIFIED - Verify before starting]` parameters, surface `"Off-Camera"` traps, and synthesize a robust architecture alongside `[OBSERVED @ MM:SS]` and `[INFERRED]` specs, `[MECHANICS]` vs. `[AESTHETICS]` separation, atomic steps, and a Deterministic Demo Seed.
4. **Stage 3 — Phased Replication Planning**: Produces a 4-milestone `implementation_plan.md` (`M1`–`M4`) with a **Robustness Architecture** section and human sign-off checkpoint for Tier 2/3 or unresolved parameters.
5. **Stage 4 — Iterative Build & Self-Verification**: Builds `M1 -> M4` sequentially inside `projects/<project-slug>/` with milestone build/lint/test checks and demo-ready seeding.
6. **Stage 5 — Multimodal Parity Audit & Delta Loop**: Compares the running replica against 5–10 Golden Keyframes (`video_MM_SS.png`) across Visual (30%), Functional (35%), Interaction (15%), and Data/Output (20%) dimensions (`>= 85` overall, `>= 90` functional), running up to 2 bounded Delta Fix Loops.

---

## Prerequisites

- **Vision-Capable LLM**: Your coding agent must support image/vision inspection for Stage 0/2 keyframes and Stage 5 screenshot parity audits.
- **Python 3.8+** (`3.10+` recommended)
- **Python packages**:
  ```bash
  pip install -r requirements.txt
  ```
- **`ffmpeg`** (for keyframe extraction):
  - **Windows**: `winget install Gyan.FFmpeg` (then restart your terminal)
  - **macOS**: `brew install ffmpeg`
  - **Linux (Debian/Ubuntu)**: `sudo apt update && sudo apt install -y ffmpeg`

Verify your environment at any time:
```bash
python .agents/skills/copycat-builder/scripts/extract_video_bundle.py --check-deps
```

---

## Usage Across Coding Agents

| Agent / Harness | Discovery Files | Notes |
| :--- | :--- | :--- |
| **Antigravity** | [AGENTS.md](AGENTS.md) + [SKILL.md](.agents/skills/copycat-builder/SKILL.md) | Native support for `<ArtifactDirectory>`, `run_workflow` fan-outs, `browser` subagent (`browser_sandbox/references/`), and `BROWSER_HOST_FACTS`. |
| **Claude Code** | [CLAUDE.md](CLAUDE.md) -> `@AGENTS.md` | Auto-imports `AGENTS.md`; stores scratch/artifacts in `.copycat_scratch/<project-slug>/`; uses native Read (vision), WebSearch, Bash, and Playwright/browser MCP. |
| **Cursor / Codex / Gemini CLI / OpenCode** | [AGENTS.md](AGENTS.md) + `.agents/skills/copycat-builder/SKILL.md` | Natively discovers `AGENTS.md` and `.agents/skills/`; falls back to `.copycat_scratch/<project-slug>/` and sequential execution when `run_workflow` is unavailable. |

Just paste a YouTube URL and ask the agent to replicate the project (for example: *"Replicate the project from https://www.youtube.com/watch?v=... "*).

---

## Repository Structure

- [AGENTS.md](AGENTS.md) — Always-on workspace rules, stage gates, and cross-agent harness fallback policy.
- [CLAUDE.md](CLAUDE.md) — Claude Code entry point importing `AGENTS.md`.
- [.agents/skills/copycat-builder/SKILL.md](.agents/skills/copycat-builder/SKILL.md) — Core 6-stage orchestrator runbook.
- [.agents/skills/copycat-builder/scripts/](.agents/skills/copycat-builder/scripts/) — `extract_video_bundle.py`, `extract_keyframes.py`, `stage_golden_frames.py`, `inspect_keyframes_workflow.py`, `research_bom_workflow.py`.
- [.agents/skills/copycat-builder/references/](.agents/skills/copycat-builder/references/) — `feasibility-rubric.md`, `domain-playbooks.md`, `parity-comparison-protocol.md`.
- [META_INSTRUCTIONS.md](META_INSTRUCTIONS.md) & [templates/INSTRUCTION_BLUEPRINT_TEMPLATE.md](templates/INSTRUCTION_BLUEPRINT_TEMPLATE.md) — Instruction-engineering blueprint and runtime artifact schemas.
