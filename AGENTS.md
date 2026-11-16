# CopyCat Builder Workspace Rules

This repository is a multi-project **CopyCat Builder** hub that analyzes YouTube project videos, evaluates replicability, extracts the underlying **skills, processes, and architectural patterns** (with examples and steps), collaborates with the user to **customize and put their own spin** on the project, and builds modular, verified implementations.

## 1. Skill Activation & Routing
- Whenever the user provides a YouTube URL or asks to replicate, clone, or copy a project from a video, **read and follow `.agents/skills/copycat-builder/SKILL.md` immediately** before doing any research, planning, or coding.

## 2. Multi-Project Workspace Layout
- Keep the repository root (`c:\Projects_MCM\CopyCat_Builder`) clean for customization rules, blueprints, and shared templates.
- Generate every replicated project inside its own isolated subfolder: `projects/<project-slug>/`.
- **Multi-Project / Multi-Idea Videos**: If a single video showcases `>= 2` distinct projects or ideas, pause after Stage 0 to list them with `[MM:SS-MM:SS]` ranges, ask the user which one(s) to build now vs. defer, create `projects/<project-slug>/PROJECT_PLACEHOLDER.md` stubs for deferred items, and execute Stages `1 -> 5` on **one selected project at a time**.
- Store all downloaded video streams, raw transcripts, manifests, and extracted keyframes under `<ArtifactDirectory>/scratch/copycat/<project-slug>/` (or `<video-id>/` when triaging a multi-project video) — **never** inside `projects/<project-slug>/` or the workspace root.

## 3. Hard Stage-Gate Discipline
Follow stages `0 -> 0.5 -> 1 -> 2A + 2 -> 2.5 -> 3 -> 4 -> 5` strictly in order (Stage 2A = External Research & Gap-Filling; Stage 2.5 = User Spin & Customization Checkpoint):
1. **No Premature Coding**: NEVER scaffold or write application source code inside `projects/<project-slug>/` before completing **Stage 1 (`feasibility_report.md` with a Tier 1–3 verdict)**, **Stage 2 (`video_reverse_spec.md` with Skills, Process & Pattern Breakdown)**, and **Stage 2.5 (`customization_brief.md`)**.
2. **Tier 4 Hard Stop**: If Stage 1 yields **Tier 4 (Non-Replicable)**, halt immediately, report documented blockers with `[MM:SS]` timestamps, and propose 1–2 closest viable software alternatives.
3. **External Research Gate (Stage 2A)**: Immediately after Stage 1 passes (Tier 1–3), use `search_web` (and the `research` subagent if needed) to find similar open-source projects, official docs, companion repos, and reference architectures. Use findings to fill parameters/setup steps the video omitted, resolve `[UNSPECIFIED - Verify before starting]` items, surface additional "Off-Camera" traps/edge cases, and synthesize a more robust architecture while preserving the video's core taught patterns. Tag every externally sourced detail `[RESEARCHED: <source/URL>]` in `video_reverse_spec.md`; research may never override an `[OBSERVED @ MM:SS]` fact. Stage 2 and 2A both must be complete before Stage 2.5, and `implementation_plan.md` must carry a "Robustness Architecture" section built from the research.
4. **User Spin & Customization Checkpoint (Stage 2.5)**: After Stage 2/2A, present the extracted **Skills, Process & Patterns** and **Customization Spin Levers** to the user and capture their domain, branding, feature twists, and pattern choices in `customization_brief.md` before Stage 3 skeletoning/planning.
5. **Human Sign-Off Checkpoint**: Require explicit user confirmation on `implementation_plan.md` before Stage 4 coding whenever **Tier 2** substitutions, **Tier 3** scoped vertical slices, or unresolved `[UNSPECIFIED - Verify before starting]` parameters exist.

## 4. Strict Reverse-Engineering Operating Rules
1. **Filter Noise**: Exclude video intros, sponsor reads, channel plugs, and narrative fluff from active specification indexing.
2. **Flag Missing Data (`[UNSPECIFIED - Verify before starting]`)**: NEVER silently guess omitted measurements, config settings, dependency versions, shader uniforms, or API payloads. Tag missing critical items `[UNSPECIFIED - Verify before starting]`; resolve via Stage 2A research (`[RESEARCHED: <source/URL>]`) or user-confirmed defaults before building. The four epistemic tags are `[OBSERVED @ MM:SS]`, `[RESEARCHED: <source/URL>]`, `[INFERRED]`, `[UNSPECIFIED - Verify before starting]`.
3. **Separate Patterns, Examples & Aesthetics**: Distinguish `[PATTERN]` (the repeatable skill/process taught) from `[VIDEO_EXAMPLE @ MM:SS]` (the creator's specific demo) and `[SPIN_LEVER]` (customizable user dimensions), alongside `[MECHANICS]` (`M2`–`M3`) vs. `[AESTHETICS]` (`M4`).
4. **Chronological & Atomic Steps**: Unpack fast-forward montages and jump-cuts into sequential, single-action steps (`[MM:SS] Action Title` with *Critical Parameter* and *Verification*).
5. **Surface "Off-Camera" Traps**: Explicitly document unshown setup/calibration prerequisites (`.env` files, CORS/proxies, seed records, fonts/assets, version locks) and bind each trap to a Milestone 1 or 2 task.
6. **Anti-Hardcoding & Stub-by-Stub Discipline**: Never hardcode an interactive project solely to the video's demo text—build a dynamic, user-customizable core tailored to `customization_brief.md` (`.env` live API support + parameterized local engine), with the video's example included only as an optional Reference Example Preset if requested. In Stage 3, scaffold modular files with documented stubs (`TODO(stub)`) first; in Stage 4, implement, document, and unit-test one stub at a time with zero unwired/dead UI controls.

## 5. Sandbox, Browser Staging & Verification Policy
- **Sandbox Isolation**: Use `BypassSandbox: true` ONLY for network-bound video/transcript fetches (`extract_video_bundle.py`), required package installs, or starting local servers that the `browser` subagent or user must reach. Keep frame extraction (`extract_keyframes.py`), builds, and unit tests inside the sandbox.
- **Browser Comparison Staging**: Before delegating Stage 5 UI/Canvas verification to the `browser` subagent (`Model: "flash"`), copy 5–10 golden keyframes (`video_MM_SS.png`) into `<ArtifactDirectory>/browser_sandbox/references/`. Always verify subagent claims against `BROWSER_HOST_FACTS` and inspect screenshots directly via `read_file`.
- **No Unverified Completion**: Never claim a build is complete until **Stage 5 (`parity_audit_report.md`)** passes all **Pattern Fidelity & User-Spin** thresholds (`>= 85` overall, `>= 90` Pattern Fidelity & User-Spin Execution) or completes the bounded 2-loop Delta Fix cycle with remaining gaps transparently reported.

## 6. Cross-Agent Harness Fallback (Non-Antigravity Harnesses)
When running in Claude Code, Cursor, Codex, Gemini CLI, OpenCode, or other non-Antigravity harnesses:
- **Artifact & Scratch Fallback**: If `<ArtifactDirectory>` is undefined, store all downloaded media, keyframes, and stage artifacts (`feasibility_report.md`, `video_reverse_spec.md`, `implementation_plan.md`, `parity_audit_report.md`) under `.copycat_scratch/<project-slug>/` (gitignored).
- **Tool Mapping**: Map Antigravity primitives to native equivalents: `read_file` (with image/vision) -> native file/image viewer; `search_web` -> web search/fetch; `run_command` -> shell/terminal tool; `browser` subagent -> Playwright/Puppeteer/browser MCP (loading reference frames directly from `.copycat_scratch/<project-slug>/frames/`); `run_workflow` -> subagents or sequential execution if parallel workflows are unavailable.
- **Vision Model Requirement**: The executing LLM **must** support image/vision inspection for Stage 0/2 keyframes and Stage 5 screenshot parity audits; if vision is unavailable, halt and notify the user.
