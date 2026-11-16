# Instruction Blueprint & Runtime Artifact Schemas

> **Purpose**: Companion structural templates for [META_INSTRUCTIONS.md](file:///c:/Projects_MCM/CopyCat_Builder/META_INSTRUCTIONS.md). Use these skeletons and schemas when generating the Antigravity `AGENTS.md` rule, the `.agents/skills/copycat-builder/SKILL.md` skill, and the three mandatory runtime artifacts produced during every video replication run.

---

## 1. Skeleton `AGENTS.md` Workspace Rule Template

> **Rule Constraint**: Must stay strictly under `24 KB` (`24,000` bytes). Keep it purely focused on routing, artifact locations, and non-negotiable stage gates.

````markdown
# CopyCat Builder Workspace Rules

When the user provides a YouTube link or requests replicating a project shown in a video:

1. **Activate Skill First**: Read and follow `.agents/skills/copycat-builder/SKILL.md` before executing any research or build steps.
2. **Stage-Gate & Missing-Data Discipline (Hard Policy)**:
   - NEVER scaffold or edit application code before completing **Stage 1 (`feasibility_report.md`)** and **Stage 2 (`video_reverse_spec.md`)**.
   - NEVER silently guess omitted measurements, settings, versions, or API payloads; tag them `[UNSPECIFIED - Verify before starting]` and resolve or confirm defaults before building.
   - Separate structural **`[MECHANICS]`** from stylistic **`[AESTHETICS]`** so core functionality builds before polish.
   - If Stage 1 yields **Tier 4 (Non-Replicable)**, STOP immediately, present documented blockers with `[MM:SS]` timestamps, and propose viable alternatives.
   - For **Tier 2** (substitutions), **Tier 3** (scoped vertical slice), or unresolved `[UNSPECIFIED]` parameters, obtain explicit user confirmation on the plan before coding.
3. **Workspace Hygiene**:
   - Store downloaded video streams, raw transcripts, and extracted keyframes under `<ArtifactDirectory>/scratch/copycat/`.
   - Keep the workspace root clean for the replicated project (or `<project-slug>/` subfolder as planned).
4. **Sandbox & Browser Staging Discipline**:
   - Use `BypassSandbox: true` ONLY for network-bound video/transcript fetches or starting a local server that the `browser` subagent or user must reach.
   - Before invoking `browser` for Stage 5 comparison, copy golden reference keyframes (`video_MM_SS.png`) to `<ArtifactDirectory>/browser_sandbox/references/` and verify subagent claims against `BROWSER_HOST_FACTS`.
5. **No Unverified Completion**: Never claim a project is replicated until **Stage 5 (`parity_audit_report.md`)** passes threshold verification or exhausts the 2-loop delta budget with remaining gaps explicitly documented.
````

---

## 2. Skeleton `.agents/skills/copycat-builder/SKILL.md` Template

> **Skill Constraint**: Valid YAML frontmatter (`name` lowercase hyphenated, third-person `description` explaining *what* and *when*), progressive disclosure links to `./scripts/` and `./references/`, and validation gates after every stage.

````markdown
---
name: copycat-builder
description: >-
  Replicate software projects demonstrated in YouTube videos through a strict
  6-stage pipeline: multimodal extraction, feasibility gating, timestamp-anchored
  reverse specification, phased planning with deterministic demo seeding,
  iterative building, and side-by-side parity auditing. Activate whenever the
  user provides a YouTube URL or asks to clone or copy a project from a video.
---

# CopyCat Builder Runbook

Follow stages `0 -> 5` strictly in order. Do not skip gates.

## Strict Operating Rules (Always Active)
1. **Filter Noise**: Ignore intros, sponsors, channel plugs, and narrative fluff; extract only technical and creative execution details.
2. **Flag Missing Data**: Never guess hidden measurements, settings, or materials. Tag omitted items `[UNSPECIFIED - Verify before starting]`.
3. **Separate Mechanics from Aesthetics**: Label structural requirements `[MECHANICS]` apart from customizable stylistic choices `[AESTHETICS]`.
4. **Chronological & Atomic**: Unpack montages and jump-cuts into sequential, single-action steps (`Critical Parameter` + `Verification`).
5. **Surface Off-Camera Traps**: Document unshown setup/calibration prerequisites and fold them into Milestone 1/2.

## Stage 0: Preflight & Multimodal Signal Extraction
1. Verify runtime tools (`python`, `yt-dlp`, `ffmpeg`). Report missing binaries before proceeding.
2. Run [extract_video_bundle.py](./scripts/extract_video_bundle.py) (`BypassSandbox: true` for network fetch) to save metadata, description links, chapters, and timestamped transcripts into `<ArtifactDir>/scratch/copycat/`.
3. Inspect description URLs first for linked GitHub repos, Gists, or documentation. Filter intro/sponsor noise intervals.
4. Run [extract_keyframes.py](./scripts/extract_keyframes.py) (sandboxed) to extract scene-change keyframes, high-density montage burst frames, and chapter fallback PNGs (`frame_MM_SS.png`), building `video_context_index.json`.
5. Inspect representative keyframes directly with `read_file`.

## Stage 1: Replicability & Feasibility Gate
1. Read [feasibility-rubric.md](./references/feasibility-rubric.md).
2. Classify the project archetype (`Web/Full-Stack`, `CLI/Backend/AI`, or `Games/Creative Coding`), state the Core Concept, identify the **Single Hardest Technical Bottleneck**, and estimate **Active Build vs. Passive/Compute Time**.
3. Audit Hard vs. Soft blockers and complete the **Bill of Materials (BOM) & Tool Substitutions** table.
4. Write `<ArtifactDir>/feasibility_report.md`.
   - **Validation Gate**: If verdict is `Tier 4 (Non-Replicable)`, STOP and present blockers + closest viable alternatives to the user.

## Stage 2: Video Understanding & Reverse Specification
1. Read [domain-playbooks.md](./references/domain-playbooks.md) (focus on the active archetype lens).
2. Inspect all key UI/code/terminal/canvas/montage keyframes via `read_file`.
3. Write `<ArtifactDir>/video_reverse_spec.md` tagging every requirement `[OBSERVED @ MM:SS]`, `[INFERRED]`, or `[UNSPECIFIED - Verify before starting]`, separating `[MECHANICS]` vs. `[AESTHETICS]`, detailing chronological atomic steps (`Critical Parameter` + `Verification`), listing `"Off-Camera" Traps & Troubleshooting`, designating 5–10 Golden Keyframes, and specifying the Deterministic Demo Seed.
   - **Validation Gate**: Every core flow and demo seed state must map to timestamps, and all `[UNSPECIFIED]` items must be surfaced before coding.

## Stage 3: Skeleton-First & Stub-by-Stub Replication Planning
1. Scaffold modular files with typed/documented stubs (`TODO(stub)`) separating the **Dynamic User Engine** from the **1-Click Video Demo Preset**.
2. Create `<ArtifactDir>/implementation_plan.md` (`RequestFeedback: true`) mapping every stub 1-to-1 to a build task + unit test across `M1`–`M4`, plus a **Robustness Architecture** section.
3. Wait for user approval before Stage 4 coding.

## Stage 4: Piece-by-Piece Stub Execution
1. Implement and document **one stub at a time** (`M1 -> M4`), running its per-stub unit/API/DOM test inside the sandbox before moving on.
2. Verify zero dead/unwired controls and verify both custom user inputs and the 1-click Demo Preset work.

## Stage 5: Dual Multimodal Parity Audit & Delta Loop
1. Read [parity-comparison-protocol.md](./references/parity-comparison-protocol.md).
2. Stage the 5–10 Golden Keyframes into `<ArtifactDir>/browser_sandbox/references/`.
3. Verify **both** Video Demo Preset parity (`browser` subagent `Model: "flash"` / CLI harness) and **Custom User-Input Functional Execution**.
4. Score parity (0–100) across Presentation, Functional, Interaction, and Data/Output dimensions in `<ArtifactDir>/parity_audit_report.md` and run up to 2 bounded Delta Fix Loops if below threshold.
````

---

## 3. Mandatory Runtime Artifact Schemas

Every CopyCat Builder instruction set must enforce the following structured outputs during execution.

### 3.0 Optional Stage 0.5 Artifact: `projects/<project-slug>/PROJECT_PLACEHOLDER.md`
> Created when a video contains `>= 2` distinct projects/ideas so deferred items can be expanded individually later without losing context.

````markdown
# Project Placeholder: <Project Title>

- **Status**: `PLACEHOLDER (Deferred — Ready for Stage 1)`
- **Project Slug**: `<project-slug>`
- **Source Video**: `<YouTube URL>` (`<Video Title>`)
- **Video Segment**: `[MM:SS - MM:SS]` (`<Chapter Name>`)
- **Primary Archetype (Preliminary)**: `Web/Full-Stack UI` | `CLI/Backend/AI` | `Games/Creative Coding`

## Concept Summary
`<1-3 sentences describing what this specific project/idea does and its observed stack/technique>`

## Stage 0 Observations & Keyframe Pointers
- **Observed Stack / Tools**: `<e.g., React, Canvas 2D, FastAPI — or [UNSPECIFIED]>`
- **Relevant Keyframes**: `<e.g., frame_03_12.png, frame_04_05.png>`
- **Transcript Notes**: `<Key quotes or requirements noted at MM:SS>`

## How to Resume Later
> Paste this prompt to expand and build this placeholder individually:
> `Expand and build projects/<project-slug>/ from its PROJECT_PLACEHOLDER.md starting at Stage 1.`
````

---

### 3.1 Runtime Artifact 1: `feasibility_report.md`

````markdown
# Feasibility & Replicability Assessment

## 1. Project Overview & Prerequisites
- **Source Video**: `<URL>` (`<Title>`, Duration: `<MM:SS>`)
- **Primary Archetype**: `Web/Full-Stack UI` | `CLI/Backend/AI` | `Games/Creative Coding`
- **Core Concept**: `<1-2 sentences on what is being built and the underlying technique>`
- **Official Source Links Found**: `<None | GitHub URL / Gist / Docs>`
- **Skill Level & Single Hardest Technical Bottleneck**: `<Intermediate/Advanced — identify the single hardest technical step in the video>`
- **Estimated Time Split**:
  - *Active Working/Build Time*: `<e.g., 45 mins>`
  - *Passive/Compute Time*: `<e.g., 10 mins (dependency install, model download, indexing, rendering)>`
- **Replicability Verdict**: `Tier 1 (Direct Replica)` | `Tier 2 (Functional Equivalent)` | `Tier 3 (Scoped Vertical Slice)` | `Tier 4 (Non-Replicable — Hard Stop)`

## 2. Bill of Materials (BOM) & Tool Substitutions
| Item / Software | Exact Spec / Setting Used | Purpose in Project | Accessible Alternative |
| :--- | :--- | :--- | :--- |
| e.g., Frontend Framework | Next.js 15, Tailwind v4 `@01:12` | Core SPA & dark dashboard UI | Same (Direct Replica) |
| e.g., LLM Provider | OpenAI `gpt-4o` JSON mode `@05:10` | Structured response streaming | `.env` key adapter + local deterministic mock replay |
| e.g., Cloud Database | Supabase Postgres `@03:45` | User records & metric storage | Local SQLite + seeded JSON fixtures |

## 3. Blocker Audit & Missing Parameter Flags
| Item | Classification | Severity | Proposed Mitigation / Parity Impact |
| :--- | :--- | :--- | :--- |
| e.g., Paid LLM API | Soft Blocker | Medium | Adapter preserves exact JSON schema and streaming UX |
| e.g., Webhook secret config | `[UNSPECIFIED - Verify before starting]` | Medium | Surface default local test secret in M1 plan |
| e.g., Custom LiDAR rig | Hard Blocker | Fatal | Triggers Tier 4 Stop (requires physical sensor) |

## 4. Scope & Go/No-Go Decision
- **In Scope**: `<Core demonstrated features>`
- **Out of Scope (if Tier 3)**: `<Deferred production extras>`
- **Gate Result**: `PROCEED` (Requires user approval of Tier 2 substitutions / `[UNSPECIFIED]` flags) | `STOP (Tier 4)`
````

---

### 3.2 Runtime Artifact 2: `video_reverse_spec.md`

````markdown
# Reverse Specification: <Project Name>

## 1. Executive Goal & Hero Moment
- **Problem Solved**: `<1-2 sentence goal>`
- **Target User**: `<Persona>`
- **Hero Moment (`[MM:SS]`)**: `<The signature workflow or visual payoff demonstrated in the video>`
- **Filtered Noise Intervals**: `<e.g., 00:00-00:25 Intro, 06:10-07:05 Sponsor Read, 14:20-15:00 Outro>`

## 2. Timestamped Scene Breakdown
| Timestamp | Chapter / Scene | Key Visual / Audio Observations | Keyframe Reference |
| :--- | :--- | :--- | :--- |
| `00:26 - 01:10` | Final Demo Reveal | Shows populated dashboard with 3 metric cards and live filter | `frame_00_32.png` |
| `01:11 - 03:15` | Architecture & Setup | Displays project folder tree & dependencies | `frame_02_10.png` |

## 2A. Skills, Process & Pattern Breakdown
- **Core Skills Taught**: `<List the underlying skills/techniques demonstrated, with [MM:SS] examples>`
- **Repeatable Process Workflow**: `<Step-by-step Input -> Transform -> Output flow independent of the creator's topic>`
- **Patterns vs. Video Example vs. Spin Levers**:
  | ID | Classification | Description | Video Example (`@ MM:SS`) | Spin Lever (How User Can Customize) |
  | :--- | :--- | :--- | :--- | :--- |
  | `PAT-01` | `[PATTERN]` | `<Core architectural/workflow pattern>` | `<Creator's demo example>` | `<Domain, UI, or rule user can swap>` |

## 3. Requirement Matrix (Epistemic & Layer Tagged)
| ID | Requirement Description | Source Tag | Classification | Priority |
| :--- | :--- | :--- | :--- | :--- |
| `REQ-01` | Dark slate sidebar (`240px`) with 4 nav items and active indicator | `[OBSERVED @ 00:32]` | `[AESTHETICS]` | High |
| `REQ-02` | Clicking "Generate" streams tokens into preview pane via SSE | `[OBSERVED @ 04:18]` | `[MECHANICS]` | Critical |
| `REQ-03` | Shader blur radius / particle damping factor (glossed over in montage) | `[UNSPECIFIED - Verify before starting]` | `[AESTHETICS]` | Medium |
| `REQ-04` | Persist edits locally in SQLite so reload retains state | `[INFERRED]` | `[MECHANICS]` | High |

## 4. Chronological & Atomic Replication Blueprint
> Group into logical phases (`Phase 1: Prep & Layout`, `Phase 2: Assembly/Core Execution`, `Phase 3: Finishing`). One atomic action per step.

### Phase 1: Prep & Layout
- **`[01:15]` Initialize project workspace & runtime dependencies:** Scaffold project directory and lock observed runtime packages (`[MECHANICS]`).
  - *Critical Parameter:* Node `20.x` / Python `3.13`, exact packages from `frame_01_22.png` (`[UNSPECIFIED - Verify before starting]` if lockfile hidden).
  - *Verification:* Clean install completes with zero peer/version errors and dev boot script starts.

### Phase 2: Assembly / Core Execution
- **`[04:18]` Implement streaming generation endpoint:** Wire `/api/generate` handler to stream chunked JSON events (`[MECHANICS]`).
  - *Critical Parameter:* `Content-Type: text/event-stream`, chunk cadence `40ms` in local mock mode (`[INFERRED]`).
  - *Verification:* `curl` or unit test receives valid sequential SSE chunks and `[DONE]` terminator.

### Phase 3: Finishing
- **`[08:40]` Apply dark glassmorphic card styling:** Match card border opacity, blur, and accent glow (`[AESTHETICS]`).
  - *Critical Parameter:* Background `#0F172A`, border `rgba(148,163,184,0.16)`, blur `12px` (`[OBSERVED @ 08:42]`).
  - *Verification:* Visual inspection against `video_08_42.png` confirms matching contrast and hierarchy.

## 5. Archetype-Specific Technical Contract
> Include the block(s) matching the active archetype:
> - **Web/Full-Stack**: Color palette hexes, font stack, route map, state transitions (`empty | loading | populated | error`), API/mock payload schemas.
> - **CLI/Backend/AI**: Command synopsis, flags, stdin/stdout ANSI layout, prompt templates, JSON schemas, exit codes.
> - **Games/Creative Coding**: Canvas resolution/aspect, loop timing (`dt`), entity state schemas, physics/collision rules, input bindings.

## 6. "Off-Camera" Traps & Troubleshooting
> Tag researched traps `[RESEARCHED: <URL>]` (Stage 2A).
| Trap ID | Likely Unshown / Glossed-Over Step | Failure Mode If Skipped | Mapped Plan Task |
| :--- | :--- | :--- | :--- |
| `TRAP-01` | Creator pre-configured `.env.local` and CORS proxy off-camera | API calls fail silently with 401/CORS errors on first click | `M1: Env & Proxy Setup` |
| `TRAP-02` | Creator pre-seeded sample records / downloaded fonts before recording | Dashboard boots blank; font fallback breaks layout parity | `M2: Deterministic Seed` |

## 7. Golden Keyframes & Deterministic Demo Seed
| Golden ID | Timestamp | Staged Filename | State / User Action to Reproduce |
| :--- | :--- | :--- | :--- |
| `GK-01` | `00:32` | `video_00_32.png` | Initial seeded landing view |
| `GK-02` | `04:18` | `video_04_18.png` | Primary workflow triggered with sample input |

- **Deterministic Seed Payload**: `<Exact JSON / SQL / fixture records or initial level parameters needed to match GK-01..GK-N>`
````

---

### 3.2B Runtime Artifact 2B: `customization_brief.md` (Stage 2.5)

````markdown
# User Spin & Customization Brief: <Project Name>

- **Source Video**: `<YouTube URL>`
- **Target Domain / Niche**: `<User's chosen topic, industry, or personal workflow>`
- **Aesthetic, Brand & Tone**: `<User's chosen naming, palette, and UX feel>`
- **Reference Example Preset**: `Custom Spin Only (Default)` | `Include Video Example Preset`

## Pattern Decisions
| Pattern ID | Video Pattern | User Decision | Custom Spin / Adaptation |
| :--- | :--- | :--- | :--- |
| `PAT-01` | `<Pattern summary>` | `Keep` / `Adapt` / `Skip` | `<How to tailor it for the user>` |

## Custom Twists & Extra Features
- `<Original feature or integration requested by the user>`
````

---

### 3.3 Runtime Artifact 3: `parity_audit_report.md`

````markdown
# Pattern Fidelity & User-Spin Verification Report

- **Project**: `<Custom Project Name>`
- **Audit Iteration**: `1 of 2` (or `2 of 2`)
- **Execution Environment**: `<URL + Viewport>` or `<CLI Command>`
- **Host Verification Source**: `BROWSER_HOST_FACTS` (Web/Canvas) | `Terminal Output Capture` (CLI/Backend)

## 1. Pattern & User-Spin Verification Matrix
| Check ID | Pattern / Spin Requirement | Live Evidence | Pattern Match | Custom Spin Match | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `PAT-01` | `<Core video pattern>` | `replica_01.png` | Pass | Pass | Executes on custom user input |
| `SPIN-01` | `<User's custom twist>` | `replica_02.png` | N/A | Pass | Tailored to `customization_brief.md` |

## 2. Quantitative Scorecard (0–100)
| Dimension | Weight | Score | Threshold | Status |
| :--- | :--- | :--- | :--- | :--- |
| Pattern & Process Fidelity | 35% | `XX` | `>= 90` | `PASS / FAIL` |
| User-Spin & Customization Execution | 35% | `XX` | `>= 90` | `PASS / FAIL` |
| UX, Polish & Adaptability | 15% | `XX` | `>= 80` | `PASS / FAIL` |
| Data, Robustness & Teachability | 15% | `XX` | `>= 85` | `PASS / FAIL` |
| **Weighted Overall Score** | **100%** | **`XX`** | **`>= 85`** | **`PASS / FAIL`** |

## 3. Delta Fix Backlog
| Delta ID | Severity | Linked Golden / Req | Observed Gap | Target Fix | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `DELTA-01` | Major | `GK-01` / `REQ-01` | Sidebar lacks badge counters shown at `00:32` | Add count badges driven by demo seed | `Fixed in Loop 1` |

## 4. Machine-Readable Summary (JSON)
```json
{
  "iteration": 1,
  "archetype": "Web/Full-Stack UI",
  "replicability_tier": "Tier 2",
  "scores": {
    "visual": 90,
    "functional": 95,
    "interaction": 85,
    "data": 90,
    "weighted_overall": 91
  },
  "critical_failures": 0,
  "verdict": "PASS"
}
```
````
