# Claude Code Instructions

@AGENTS.md

## Claude Code Skill Discovery
- Activate [SKILL.md](.agents/skills/copycat-builder/SKILL.md) whenever the user provides a YouTube URL or asks to replicate, clone, or copy a project from a video.
- Use the **Cross-Agent Harness Fallback** rules in `AGENTS.md` and `SKILL.md`: store artifacts and scratch media under `.copycat_scratch/<project-slug>/`, use native file/image reading (vision required), WebSearch, shell execution, and Playwright/browser MCP (or sequential fallback for `run_workflow`).
