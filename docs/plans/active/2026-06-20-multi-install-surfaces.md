# Development Plan

Make the repository expose one canonical interface-design skill through multiple equal install surfaces. No supported tool is ranked above another. Codex, Claude Code, and any future tool are install surfaces generated from neutral package metadata and shared skill content.

## Task intake

- Goal: implement "one design skill, several install surfaces" without ranking tools.
- Context: current work introduced Codex/Claude surfaces but still carried tool-ranking wording. The repository needs a neutral model where adding another tool means adding another install surface.
- Constraints: do not rank supported tools; keep shared skill content under `skills/`; keep package metadata in a tool-neutral manifest; generated install surfaces must not drift; every bundled `SKILL.md` needs `name` and `description`.
- Done when: neutral metadata exists, Codex and Claude surfaces are generated from the same sources, docs and `AGENTS.md` avoid tool hierarchy, checks pass, and search finds no tool-ranking language in repo instructions or install-surface metadata.
- Saved plan: `docs/plans/active/2026-06-20-multi-install-surfaces.md`

## Scope

### In scope

- [x] Add neutral package metadata in `install-surfaces/manifest.json`.
- [x] Keep `skills/interface-design` as canonical skill content.
- [x] Generate Codex metadata from the neutral manifest.
- [x] Generate Claude Code metadata and command wrappers from the same sources.
- [x] Update README and AGENTS to describe equal install surfaces.
- [x] Verify generated surfaces, JSON, skill frontmatter, and absence of tool-ranking language.

### Out of scope

- [ ] Publish external packages or registries.
- [ ] Add a third install surface in this change.
- [ ] Change product-design behavior beyond neutralizing tool-specific invocation wording.

## Assumptions

- It is acceptable for tool-specific files to exist in tool-specific paths, as long as they are generated from shared sources and not described as more important than other surfaces.
- `skills/interface-design-*` can remain as local entrypoints, while `skills/interface-design` remains the main design skill content.
- A dependency-free Python script is sufficient for sync and verification.

## Execution protocol

For every step:

1. Define the expected result.
2. Implement only that step.
3. Verify with concrete commands or inspection.
4. Fix immediately if verification fails.
5. Mark complete only after implementation and verification both pass.

## Step 1: Neutralize the architecture

### Objective

Replace tool hierarchy with neutral canonical sources and generated install surfaces.

### Expected result

The repository model is: `install-surfaces/manifest.json` for shared package metadata, `skills/interface-design` for skill content, generated files for each tool surface.

### Implementation checklist

- [x] Add `install-surfaces/manifest.json`.
- [x] Update `scripts/sync-install-surfaces` to generate `.codex-plugin/plugin.json`.
- [x] Update `scripts/sync-install-surfaces` to generate `.agents/plugins/marketplace.json`.
- [x] Keep Claude Code surface generation in the same script.
- [x] Make generated descriptions avoid tool-ranking language.

### Verification checklist

- [x] `scripts/sync-install-surfaces` succeeds.
- [x] Generated metadata versions match `install-surfaces/manifest.json`.
- [x] Generated surfaces are synchronized in `scripts/sync-install-surfaces --check`.

### Completion evidence

- Expected: neutral manifest drives all tool-specific metadata.
- Implemented: `install-surfaces/manifest.json`; script generation for Codex, `.agents`, and Claude surfaces.
- Verified by: `scripts/sync-install-surfaces` and `scripts/sync-install-surfaces --check`.
- Quality result: passed.
- Fixes applied: moved version comparison from Codex metadata to neutral manifest.

## Step 2: Neutralize skill and docs wording

### Objective

Remove tool-ranking and tool-exclusive wording from canonical skill content, generated surfaces, README, and AGENTS.

### Expected result

Docs describe Codex and Claude Code as install surfaces; no supported tool is privileged over another.

### Implementation checklist

- [x] Update `skills/interface-design/SKILL.md` to say "coding agents" and "install surfaces".
- [x] Update `skills/interface-design/references/imagegen.md` to avoid Codex-only wording.
- [x] Regenerate `.claude/skills/interface-design/*`.
- [x] Rewrite README around equal install surfaces.
- [x] Rewrite AGENTS expectations around install-surface neutrality.
- [x] Keep `.githooks/pre-commit` using check mode.

### Verification checklist

- [x] `scripts/sync-install-surfaces --check` passes.
- [x] README and AGENTS do not instruct contributors to privilege one tool over another.
- [x] Generated Claude skill mirror matches `skills/interface-design`.

### Completion evidence

- Expected: no tool hierarchy in repository instructions or generated install metadata.
- Implemented: neutral README/AGENTS wording, neutral skill invocation wording, regenerated surfaces.
- Verified by: targeted text search for hierarchy terms, `diff -rq skills/interface-design .claude/skills/interface-design`, and sync check.
- Quality result: passed.
- Fixes applied: replaced "Codex Invocation" with "Invocation Across Install Surfaces"; replaced "Codex image generation" with general image generation wording.

## Step 3: Final validation

### Objective

Prove the requested end state from current files.

### Expected result

Generated surfaces are synchronized; metadata parses; frontmatter is valid; search does not find tool hierarchy language where it would define repo policy or metadata.

### Implementation checklist

- [x] Run `scripts/sync-install-surfaces --check`.
- [x] Parse all metadata JSON.
- [x] Check generated Claude mirror against canonical skill.
- [x] Search docs, script, manifest, and metadata for old hierarchy wording.
- [x] Inspect Git status and diff.

### Verification checklist

- [x] `scripts/sync-install-surfaces --check`.
- [x] `python3 -m json.tool` for `install-surfaces/manifest.json`, `.codex-plugin/plugin.json`, `.agents/plugins/marketplace.json`, `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`.
- [x] `diff -rq skills/interface-design .claude/skills/interface-design`.
- [x] Conflict-marker search.
- [x] Git status inspection.

### Completion evidence

- Expected: repository has one canonical design skill and equal generated install surfaces.
- Implemented: neutral manifest, generated Codex and Claude install surfaces, neutral docs/instructions, sync/check tooling.
- Verified by: final validation commands listed above.
- Quality result: passed.
- Fixes applied: none after final validation.

## Validation plan

- [x] Typecheck/build: not applicable; repository has no package manager or compiled code.
- [x] Static verification: `scripts/sync-install-surfaces --check`.
- [x] JSON checks: all manifest/plugin/marketplace JSON files parse.
- [x] Integration checks: generated Claude skill mirror matches canonical skill content.
- [x] Regression checks: old tool hierarchy language removed from repo instructions and generated metadata.

## Risks and edge cases

- [x] A future install surface becomes privileged by accident.
  - Mitigation: AGENTS says the repo is install-surface neutral and forbids ranking supported tools.
  - Verification: text search and README inspection.
- [x] Generated metadata drifts from neutral manifest.
  - Mitigation: sync/check script compares generated files to expected content.
  - Verification: `scripts/sync-install-surfaces --check`.
- [x] Tool-specific generated files are edited by hand.
  - Mitigation: README and AGENTS point maintainers to canonical sources and sync command.
  - Verification: generated file check mode fails on drift.

## Final Definition of Done

- [x] Tool-neutral package manifest exists.
- [x] Canonical skill content is shared.
- [x] Codex install surface exists.
- [x] Claude Code install surface exists.
- [x] No supported tool is ranked above another.
- [x] Checks pass.
