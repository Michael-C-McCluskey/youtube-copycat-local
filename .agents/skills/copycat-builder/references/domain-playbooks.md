# Stage 2 Reference: Domain Playbooks & Reverse-Engineering Lenses

Use this playbook during **Stage 2** to convert extracted keyframes, transcripts, and linked code into a rigorous `<ArtifactDir>/video_reverse_spec.md` without guessing hidden parameters or mixing mechanics with cosmetic polish.

---

## 1. Mandatory Epistemic & Layer Rules

### A. 4-Tier Epistemic Tagging Contract
Every requirement, component, schema field, measurement, or parameter in `video_reverse_spec.md` MUST carry one tag:
- **`[OBSERVED @ MM:SS]`**: Directly seen in a keyframe, file tree, code snippet, or terminal output, or stated verbatim in the transcript.
- **`[RESEARCHED: <source/URL>]`**: Detail sourced externally during Stage 2A (official docs, companion repo, similar open-source project, reference architecture). Must cite a concrete URL. May fill gaps or add robustness but NEVER contradicts or overrides an `[OBSERVED]` fact.
- **`[INFERRED]`**: Standard, low-risk glue logic (e.g., Express error middleware, CORS headers, helper serialization) that does not invent core specs and never contradicts an `[OBSERVED]` fact.
- **`[UNSPECIFIED - Verify before starting]`**: Critical measurement, package version, color/shader constant, prompt template, API payload field, or runtime setting omitted/blurred/cut by the creator **and not resolvable by research**. **Never silently guess these.** Surface every remaining `[UNSPECIFIED]` item explicitly in `video_reverse_spec.md` and `implementation_plan.md` so the user can provide it or approve a documented default.

### B. Mechanics vs. Aesthetics Separation
Classify every requirement and blueprint step as either:
- **`[MECHANICS]`**: Structural logic, data schemas, state transitions, CLI arguments, physics/collision loops, and dependencies required for the project to operate. (Built in **Milestones 2 & 3**.)
- **`[AESTHETICS]`**: Palette hex codes, fonts, shadows, glassmorphism, animations, ASCII styling, particle colors, or customizable presentation choices. (Refined in **Milestone 4**.)

### C. Chronological & Atomic Montage Unpacking
Creators frequently compress 10 edits into a 15-second montage or timelapse. Unpack montages and complex sequences into **sequential, single-action steps** grouped into logical phases (`Phase 1: Prep & Layout`, `Phase 2: Assembly / Core Execution`, `Phase 3: Finishing`):
- **`[MM:SS] Action Title`** (`[MECHANICS]` | `[AESTHETICS]`): Clear imperative instruction.
  - *Critical Parameter:* Exact measurement, version, flag, coordinate, ratio, or `[UNSPECIFIED - Verify before starting]`.
  - *Verification:* Concrete check confirming this single step succeeded before moving on.

---

## 2. Archetype Extraction Lenses

Apply the lens matching the project's primary archetype (or multiple lenses for hybrids):

### Lens A: Web Apps, UIs & Full-Stack Demos
1. **Layout & Spatial Hierarchy**: Shell pattern (sidebar width, top navbar height, split panes, grid columns, responsive collapse behavior).
2. **Design Tokens (`[AESTHETICS]`)**: Sample background, surface, border, primary/secondary accent hex colors from Golden Keyframes; font families, weights, radii, shadows, blur/glass values.
3. **Component & Route Inventory**: Every visible page, route URL, card, modal, drawer, toast, and empty/loading/error state.
4. **State Machine & API Contracts (`[MECHANICS]`)**: Client state vs. server state, optimistic updates, streaming/SSE/WebSocket events, request/response JSON shapes.
5. **Motion & Micro-Interactions (`[AESTHETICS]`)**: Hover transforms, modal entry transitions, drag-and-drop physics, scroll-triggered effects.

### Lens B: CLI Tools, Scripts, Backend Services & AI/ML Workflows
1. **CLI Signature (`[MECHANICS]`)**: Binary/entry command, subcommands, positional args, `--flags`, defaults, environment variables, exit codes.
2. **Terminal Presentation (`[AESTHETICS]`)**: Spinners, progress bars, Rich/ANSI color tables, boxed banners, streaming token cadence.
3. **Pipeline & Data Flow (`[MECHANICS]`)**: Input sources, chunking/parsing stages, retry/backoff rules, concurrency model, output files or stdout formats.
4. **AI / Prompt Contracts**: Model names/tiers, system/user prompt structure, structured JSON output schemas, tool definitions, fallback/mock behavior.
5. **Backend Endpoints**: HTTP methods, paths, query/body schemas, status codes, middleware (auth, rate limits, CORS).

### Lens C: Games, Animations & Creative Coding
1. **Canvas & Render Pipeline**: Resolution/aspect ratio, HiDPI (`devicePixelRatio`) scaling, renderer (Canvas 2D, Three.js/WebGL, P5.js, SVG, Pygame), camera projection/FOV.
2. **Core Loop & Timing (`[MECHANICS]`)**: Fixed vs. variable `dt` timestep, FPS cap, physics integration, collision geometry (AABB, circle, raycast), win/loss/score rules.
3. **Entity & Scene State**: Spawn tables, entity properties (position, velocity, HP, cooldowns), state machine (`Title -> Playing -> Paused -> GameOver`).
4. **Controls & Audio**: Exact keybindings, mouse/pointer drag/aim behavior, touch support, WebAudio/sound trigger events.
5. **Visual Craft (`[AESTHETICS]`)**: Shaders/uniforms, particle lifetimes, easing functions, screen-shake, HUD typography/placement.

---

## 3. "Off-Camera" Traps & Troubleshooting Checklist

Creators routinely edit out unglamorous prep steps. Always audit for these hidden traps and convert any relevant ones into `M1`/`M2` tasks:

| Archetype | Common "Off-Camera" Traps |
| :--- | :--- |
| **Web / Full-Stack** | `.env.local` keys pre-populated; CORS/Vite proxy configured; DB migrations & seed scripts already run; custom Google Fonts or icon packs installed; Tailwind v3 vs. v4 config differences. |
| **CLI / Backend / AI** | Python virtualenv / Poetry / `uv` activated; `PYTHONUTF8=1` on Windows for Unicode/emoji Rich tables; local sample input fixtures (`sample.pdf`, `data.csv`) placed in CWD; API base URL overrides. |
| **Games / Creative Coding** | Preloaded sprites/fonts/audio assets blocking first frame; `devicePixelRatio` blur fix; `requestAnimationFrame` delta clamping on tab switch; initial random seed / entity pool warm-up. |

---

## 4. Golden Keyframes & Deterministic Demo Seed

1. **Golden Keyframes (5–10 frames)**: Select frames covering:
   - `GK-01`: Initial booted/seeded landing screen or CLI help/banner.
   - `GK-02..GK-04`: Core user actions in progress (form submit, drag, CLI run, active gameplay).
   - `GK-05`: Edge/error/empty/toggle state (if demonstrated).
   - `GK-06+`: Signature "Hero Moment" output state.
2. **Deterministic Demo Seed Specification**: Document the exact initial JSON records, SQLite rows, input files, or level parameters so `projects/<project-slug>/` boots immediately into `GK-01` without manual tinkering.

---

## 5. Stage 2A: External Research Synthesis Rules

Run right after Stage 1 passes (Tier 1–3); merge results into the Stage 2 `video_reverse_spec.md`. Goal: fill parameters and setup steps the video omitted and harden the architecture **without changing anything the video demonstrates** (full visual and functional parity).

### Research Queries & Selective Fan-Out
- **Threshold**: When there are `>= 4` BOM items or `>= 3` `[UNSPECIFIED - Verify before starting]` items, run `run_workflow` with `scripts/research_bom_workflow.py` to fan out in parallel across items. Otherwise, run `search_web` directly (or delegate broad surveys to `research`).
- **Frame Inspection Threshold**: When `frames_extracted >= 15` in `video_context_index.json`, run `run_workflow` with `scripts/inspect_keyframes_workflow.py` instead of sequential `read_file` calls.
- **Core Queries**:
  1. `"<core technique>" open source example github` — similar projects and reference implementations.
  2. Official docs for every BOM item (exact versions, config keys, API shapes, breaking changes since the video's upload date).
  3. Creator's companion repo/gist/blog (check `description_links` in `video_manifest.json` first).
  4. `"<framework> <technique>" common pitfalls / gotchas` — hidden traps.

### Use of Findings
| Purpose | Rule |
| :--- | :--- |
| Resolve `[UNSPECIFIED]` | Replace with the researched value and tag `[RESEARCHED: <URL>]`. If sources conflict or none exist, keep `[UNSPECIFIED - Verify before starting]` and ask the user. |
| Find Off-Camera Traps | Add to the `"Off-Camera" Traps` table with `[RESEARCHED: <URL>]` and bind to `M1`/`M2`. |
| Robust architecture | Add validation, error handling, config management, tests, pinned versions, accessibility, or security fixes. Tag `[RESEARCHED: <URL>]` or `[INFERRED]`. |
| Version drift | If the video's libraries are outdated/broken today, record the working version and the reason. |

### Fidelity Guardrails
- Robustness additions must be **invisible** to demonstrated UX, visuals, outputs, and timing at every Golden Keyframe.
- Never swap a framework/library the video shows (`[OBSERVED]`) for a "better" one unless it is unavailable (then escalate as a Tier 2 substitution).
- Do not copy code verbatim from external repos without noting license; prefer reimplementing from documented patterns.

### Output: `## Research Synthesis` section in `video_reverse_spec.md`
| Finding | Source URL | Tag Applied | Impact (Resolved Param / New Trap / Architecture Upgrade) | Affects Milestone |
| :--- | :--- | :--- | :--- | :--- |

---

## 6. Pattern vs. Video Example, Spin Levers & Stub Contract Rules

1. **Distinguish `[PATTERN]`, `[VIDEO_EXAMPLE @ MM:SS]`, and `[SPIN_LEVER]`**:
   - **`[PATTERN]` (Invariant Skill / Process)**: The repeatable technique, architecture, or `Input -> Transform -> Output` workflow the video teaches (e.g., *skill-based prompt pipeline*, *procedural canvas telemetry*, *3-way A/B scoring*).
   - **`[VIDEO_EXAMPLE @ MM:SS]`**: The creator's concrete demo topic, dataset, or branding used to illustrate the pattern.
   - **`[SPIN_LEVER]`**: Any dimension the user can customize at **Stage 2.5**—target niche/domain, aesthetic/branding, data sources, or extra feature twists.
   - Apply this across all three archetype lenses (**Web/Full-Stack UI**, **CLI/Backend/AI**, **Games/Creative Coding**): document *how* the pattern works and *how* the user can adapt it, not just what pixels appear on screen.
2. **Separate Dynamic Engine from Optional Reference Example**:
   - Never hardcode business logic, generators, or UI outputs solely to the video's example.
   - Build a **User-Parameterized Core** tailored to `customization_brief.md` (accepting custom user inputs + optional `.env` API keys with a real dynamic local fallback), and include the video's showcase only as an optional **Reference Example Preset** if requested at Stage 2.5.
3. **Skeleton-First Stub Contract (`Stage 3A -> Stage 4`)**:
   - Keep modules small and single-purpose (`<= 250–300` lines each).
   - Every stub created in Stage 3A must define:
     - **File & Symbol**: `module.py::func_name` or `view.js::renderView`
     - **Taught Pattern & Spin**: Which `[PATTERN]` / `customization_brief.md` requirement it implements
     - **Inputs / Outputs / Error Handling**: Explicit types and failure behavior
     - **Per-Stub Verification Test**: Exact unit/API/DOM assertion to pass before moving to the next stub.
   - Zero dead buttons, placeholder alerts, or unwired controls may remain after Stage 4.
