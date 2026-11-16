---
name: copycat-builder
description: >-
  Extract the skills, repeatable processes, and architectural patterns taught in
  YouTube project videos, collaborate with the user to customize them with their
  own spin, and build modular verified projects through a strict pipeline. Use
  whenever the user provides a YouTube URL or asks to replicate, learn from,
  customize, or build a project inspired by a video.
---

# CopyCat Builder Orchestrator Runbook

Execute stages `0 -> 0.5 -> 1 -> 2A + 2 -> 2.5 -> 3 -> 4 -> 5` strictly in order for a given YouTube URL and target `<project-slug>`. Keep all downloaded media and extracted frames in `<ArtifactDir>/scratch/copycat/<project-slug>/` and generate all project source code inside `projects/<project-slug>/`.

Follow [INSTRUCTION_BLUEPRINT_TEMPLATE.md](../../../templates/INSTRUCTION_BLUEPRINT_TEMPLATE.md) for exact output schemas of `feasibility_report.md`, `video_reverse_spec.md`, and `parity_audit_report.md`.

## Strict Operating Rules (Always Active)
1. **Filter Noise**: Exclude intros, sponsors, channel plugs, and narrative filler (`[NOISE @ MM:SS-MM:SS]`).
2. **Flag Missing Data**: Never silently guess hidden measurements, versions, settings, or payloads. Tag omitted critical parameters `[UNSPECIFIED - Verify before starting]`; resolve via Stage 2A research (`[RESEARCHED: <source/URL>]`) or user confirmation.
3. **Separate Mechanics from Aesthetics**: Classify structural requirements (`[MECHANICS]`) apart from customizable styling (`[AESTHETICS]`).
4. **Chronological & Atomic**: Unpack montages and jump-cuts into sequential, single-action steps (`[MM:SS] Action Title` + *Critical Parameter* + *Verification*).
5. **Surface Off-Camera Traps**: Identify unshown prerequisites (env vars, CORS/proxy setup, seed data, asset preloading) and bind them into Milestone 1 or 2.

---

## Stage 0: Preflight & Multimodal Signal Extraction
1. **Preflight Dependencies**: Run `python .agents/skills/copycat-builder/scripts/extract_video_bundle.py --check-deps` inside the sandbox to inspect `yt-dlp`, `youtube-transcript-api`, and `ffmpeg`. Report or install missing tools before proceeding.
2. **Extract Metadata, Links & Transcript**: Run [extract_video_bundle.py](./scripts/extract_video_bundle.py) (`BypassSandbox: true` only for network fetch) with `--download-video` targeting `<ArtifactDir>/scratch/copycat/<project-slug>/`.
3. **Linked-Asset Triage**: Inspect `video_manifest.json` (`description_links`, `resolved_description_links`, and downloaded files in `linked_resources/`) first for official blogs, GitHub repos, Gists, downloadable `.md` skills, PDFs, or prompts. Record any linked source code/assets, and flag intro/sponsor noise intervals.
4. **Extract Scene, Montage & Chapter Keyframes**: Run [extract_keyframes.py](./scripts/extract_keyframes.py) (sandboxed) against the downloaded `.mp4` and `video_manifest.json` to produce `frame_MM_SS.png` files and `video_context_index.json`.
5. **Multimodal Inspection**: Read `video_context_index.json`. If `frames_extracted >= 15`, run `run_workflow` using [inspect_keyframes_workflow.py](./scripts/inspect_keyframes_workflow.py) (`Args={"index_path": "<path-to-video_context_index.json>"}`); if `< 15`, inspect the keyframe PNGs directly in small batches via `read_file`.
   - **Exit Gate**: `video_manifest.json`, `video_context_index.json`, and representative frames inspected.

---

## Stage 0.5: Multi-Project / Multi-Idea Triage (When Video Has `>= 2` Projects)
If the video showcases **multiple distinct projects or ideas** (e.g., listicles, "5 apps in 10 minutes", multi-demo showcases):
1. **Inventory All Projects**: List each project/idea with its `[MM:SS-MM:SS]` range, title, 1-line summary, and proposed `<project-slug>`.
2. **Ask the User to Pick**: Use `ask_question` (`is_multi_select: true`, or plain-text selection in non-Antigravity harnesses) to ask which project(s) to **build now** vs. **save as placeholders** for later. Recommend picking **1** project to build first to stay focused.
3. **Create Placeholders for Deferred Items**: For every deferred project/idea (and any queued selection not yet started), create `projects/<project-slug>/PROJECT_PLACEHOLDER.md` using the schema in [INSTRUCTION_BLUEPRINT_TEMPLATE.md](../../../templates/INSTRUCTION_BLUEPRINT_TEMPLATE.md) (recording source URL, `[MM:SS-MM:SS]` slice, concept summary, observed stack/frames, and a 1-line resume prompt).
4. **Focus Rule**: Execute Stages `1 -> 2A + 2 -> 3 -> 4 -> 5` for **one selected project at a time** before moving to the next.
   - *(When resuming a `PROJECT_PLACEHOLDER.md` later, reuse cached Stage 0 media if present or re-extract targeted `[MM:SS-MM:SS]` frames, then start at Stage 1.)*

---

## Stage 1: Replicability & Feasibility Gate
1. Read [feasibility-rubric.md](./references/feasibility-rubric.md).
2. Summarize the **Core Concept**, identify the **Single Hardest Technical Bottleneck**, and estimate **Active Working vs. Passive/Compute Time**.
3. Classify the project archetype: **Web/Full-Stack UI**, **CLI/Backend/AI**, or **Games/Creative Coding**.
4. Audit Hard vs. Soft blockers and complete the **Bill of Materials (BOM) & Tool Substitutions** table.
5. Assign a **Replicability Verdict** (`Tier 1 Direct Replica`, `Tier 2 Functional Equivalent`, `Tier 3 Scoped Vertical Slice`, or `Tier 4 Non-Replicable Hard Stop`) and write `<ArtifactDir>/feasibility_report.md`.
   - **Validation Gate**: If verdict is `Tier 4`, **STOP immediately**, present `[MM:SS]`-anchored blockers, and propose 1–2 closest viable Tier 2/3 alternatives.

---

## Stage 2: Deep Video Understanding & Reverse Specification
1. Read [domain-playbooks.md](./references/domain-playbooks.md) and apply the checklist for the active archetype lens.
2. Inspect remaining montage, code, terminal, and UI state keyframes (reuse `inspect_keyframes_workflow.py` output when `frames_extracted >= 15`, or `read_file` directly when `< 15`).
3. Write `<ArtifactDir>/video_reverse_spec.md` containing:
   - Filtered noise intervals and timestamped scene breakdown.
   - **`## Skills, Process & Pattern Breakdown`**:
     - **Core Skills Taught**: Techniques and capabilities the creator demonstrates, with `[MM:SS]` examples.
     - **Repeatable Process Workflow**: Step-by-step `Input -> Transform -> Output` pattern independent of the video's specific topic/brand.
     - **Transferable Patterns & Spin Levers**: Explicitly distinguish `[PATTERN]` (invariant mechanism) from `[VIDEO_EXAMPLE @ MM:SS]` (creator's demo) and `[SPIN_LEVER]` (customizable user dimensions).
   - Requirement Matrix tagged `[OBSERVED @ MM:SS]`, `[INFERRED]`, or `[UNSPECIFIED - Verify before starting]` and classified `[MECHANICS]` vs. `[AESTHETICS]`.
   - Chronological & Atomic Blueprint (`Phase 1`..`Phase 3` with *Critical Parameter* and *Verification*).
   - `"Off-Camera" Traps & Troubleshooting` table.
   - Archetype-specific contract, **5–10 Golden Keyframes**, and **Reference Example Seed** payload.
   - **Validation Gate**: Zero silent guesses for critical parameters; every core pattern and example maps to `[MM:SS]` evidence.

---

## Stage 2A: External Research & Gap-Filling
Entry: Stage 1 verdict is Tier 1–3 (run right after Stage 1, before/alongside drafting Stage 2; results are merged into `video_reverse_spec.md`).
1. If there are `>= 4` BOM items or `>= 3` `[UNSPECIFIED - Verify before starting]` items, run `run_workflow` using [research_bom_workflow.py](./scripts/research_bom_workflow.py) (`Args={"project_concept": ..., "bom_items": [...], "unspecified_items": [...]}`). Otherwise, use `search_web` directly (or the `research` subagent) to find similar open-source projects, official docs, companion repos, and reference architectures.
2. Use findings to: (a) resolve `[UNSPECIFIED - Verify before starting]` parameters (versions, configs, constants); (b) discover additional `"Off-Camera" Traps` and edge cases; (c) synthesize a **more robust, production-grade architecture** (error handling, validation, config, tests) while preserving 100% fidelity to the video's demonstrated UX/behavior.
3. Tag every externally sourced detail `[RESEARCHED: <source/URL>]` alongside `[OBSERVED @ MM:SS]`, `[INFERRED]`, and `[UNSPECIFIED - Verify before starting]`. Research may never override or alter an `[OBSERVED]` requirement; robustness additions must be invisible to the demonstrated behavior.
4. Append a `## Research Synthesis` section to `<ArtifactDir>/video_reverse_spec.md` listing sources, resolved parameters, newly found traps, and architecture upgrades (see [domain-playbooks.md](./references/domain-playbooks.md) Section 5).
   - **Validation Gate**: Every resolved parameter cites a source URL; remaining `[UNSPECIFIED]` items are listed for user confirmation in Stage 2.5/3.

---

## Stage 2.5: User Spin & Customization Checkpoint (Mandatory Pause)
Before creating the code skeleton or implementation plan, help the user make the project their own:
1. **Present What the Video Teaches**: Summarize the **Core Skills**, **Repeatable Process Steps**, and **Customization Spin Levers** extracted in Stage 2 (with brief `[MM:SS]` examples).
2. **Capture the User's Spin**: Use `ask_question` (or structured prompts in non-Antigravity harnesses) to align on:
   - **Target Domain / Niche / Use Case**: What topic, industry, or personal workflow will this serve?
   - **Aesthetic, Brand & Tone**: What visual theme, naming, or UX feel does the user want?
   - **Patterns to Keep, Adapt, or Skip + Custom Twists**: Which video skills/steps to keep as-is, modify, omit, or extend with new features.
   - **Reference Example Preference**: Seed 100% around the user's custom spin (default), or also include an optional `"Video Reference Example"` preset toggle.
3. **Write Customization Brief**: Save `<ArtifactDir>/customization_brief.md` following [INSTRUCTION_BLUEPRINT_TEMPLATE.md](../../../templates/INSTRUCTION_BLUEPRINT_TEMPLATE.md). Do not proceed to Stage 3 until the user's spin is recorded.

---

## Stage 3: Skeleton-First & Stub-by-Stub Planning
1. **Stage 3A — Architecture Skeleton & Typed Stubs**: Scaffold the modular directory layout inside `projects/<project-slug>/` with documented interface stubs (`TODO(stub)` / docstrings / input-output contracts) organized around the **extracted Process & Patterns** and **`customization_brief.md`** (never write monolithic `>300`-line files or hardcode the app to the video's example).
2. **Stage 3B — Granular Stub-by-Stub Task Plan**: Create `<ArtifactDir>/implementation_plan.md` (`RequestFeedback: true`) mapping every stub 1-to-1 to an atomic build task + exact per-stub verification test across 4 milestones:
   - `M1`: Workspace, `.env` Config, BOM Dependencies, `"Off-Camera"` Traps & `[RESEARCHED]` robustness upgrades
   - `M2`: Core Pattern Engine, Live API / Local Parameterized Adapter & User-Spin Seed (`[MECHANICS]`)
   - `M3`: Modular User Flows, Custom Twists & Interactive Controls (`[MECHANICS]` traced to `[PATTERN]` & `customization_brief.md`)
   - `M4`: User-Chosen Branding, Visual Polish & Animations (`[AESTHETICS]`)
3. Include a **Robustness Architecture** section synthesizing every `[RESEARCHED: <source>]` item by milestone while preserving the core taught patterns.
4. Present any Tier 2 substitutions, Tier 3 scope cuts, or `[UNSPECIFIED]` defaults for explicit user approval before Stage 4.

---

## Stage 4: Piece-by-Piece Stub Execution & Per-Stub Verification
1. Implement **one stub at a time** in `M1 -> M4` order inside `projects/<project-slug>/`, keeping each module small, single-purpose, and documented (explicitly noting which taught skill/pattern the stub implements).
2. After completing each stub, run its dedicated unit/API/DOM test inside the sandbox (`BypassSandbox: false`) before moving to the next stub.
3. Verify zero dead or unwired buttons/inputs exist and that the app executes end-to-end on the user's custom spin inputs (plus the optional Reference Example Preset if included).

---

## Stage 5: Pattern Fidelity & User-Spin Verification
1. Read [parity-comparison-protocol.md](./references/parity-comparison-protocol.md).
2. **Stage Reference Keyframes (When Comparing Mechanics)**: Run [stage_golden_frames.py](./scripts/stage_golden_frames.py) to copy relevant Golden Keyframes (`video_MM_SS.png`) into `<ArtifactDir>/browser_sandbox/references/`.
3. **Run Dual Empirical Verification**:
   - *Part 1 — Pattern & Process Fidelity*: Verify every core skill, workflow step, and architectural pattern from `video_reverse_spec.md` executes end-to-end (comparing against `video_MM_SS.png` for structural/process mechanics, or full visual parity only if the user requested a 1:1 visual clone).
   - *Part 2 — User-Spin Execution*: Test the running app (`invoke_subagent(TypeName="browser", Model="flash")` + `BROWSER_HOST_FACTS`, or CLI trace harness) against every custom requirement in `customization_brief.md` and fresh user inputs (zero dead controls or hardcoded creator text).
4. **Score & Iterate**: Write `<ArtifactDir>/parity_audit_report.md` using the 4-dimension 0–100 rubric (Pattern & Process Fidelity 35%, User-Spin Execution 35%, UX & Adaptability 15%, Data & Robustness 15%).
5. If overall score `< 85`, Pattern Fidelity `< 90`, User-Spin Execution `< 90`, or any critical pattern/spin test fails, run up to **2 bounded Delta Fix Loops** (`Stage 4 -> Stage 5`), then finalize `walkthrough.md`.

---

## Cross-Agent Harness Fallback
- **Vision Requirement**: The executing model must support image/vision inspection for Stage 0/2 keyframes and Stage 5 visual parity audits.
- **Storage Fallback**: Where `<ArtifactDir>` is not provided by the host harness, read/write all scratch media, extracted frames, and runtime artifacts under `.copycat_scratch/<project-slug>/`.
- **Tool Equivalents**:
  - `read_file` -> host file/image reader (vision-enabled)
  - `search_web` / `research` -> host web search / fetch tool
  - `run_command` -> host terminal/bash/PowerShell execution
  - `run_workflow` -> host parallel subagents, or sequential batching over `extract_keyframes.py` / BOM items
  - `browser` subagent -> host Playwright/Puppeteer/browser MCP comparing live screenshots against `.copycat_scratch/<project-slug>/frames/frame_MM_SS.png`
