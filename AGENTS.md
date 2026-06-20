# AGENTS.md

## Repository Expectations

- This repository is install-surface neutral. Do not rank supported tools or describe one tool as privileged over another.
- Canonical package metadata lives in `install-surfaces/manifest.json`.
- Canonical skill content lives under `skills/`.
- Tool-specific install surfaces live under paths such as `.codex-plugin/`, `.agents/plugins/`, `.claude/`, and `.claude-plugin/`.
- Generated install-surface files are synchronized by `scripts/sync-install-surfaces`; do not edit generated files by hand.
- Every bundled skill must have a `SKILL.md` with `name` and `description` frontmatter.
- Keep skill descriptions concise and scoped. They are the trigger contract Codex sees before loading the full skill.
- Prefer references under `skills/<skill>/references/` for detailed design guidance instead of bloating `SKILL.md`.
- After changing canonical skill content or install-surface metadata, run `scripts/sync-install-surfaces` and `scripts/sync-install-surfaces --check`.
