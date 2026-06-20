# Interface Design

Interface design skills for dashboards, admin panels, SaaS apps, tools, settings pages, data interfaces, and interactive products.

Interface Design helps coding agents make UI choices from product intent instead of generic dashboard defaults. It also keeps decisions in `.interface-design/system.md` so future UI work can reuse the same spacing, depth, tokens, component patterns, and visual direction.

The repository uses **one canonical design skill** and publishes it through multiple install surfaces. No supported tool is ranked above another. Codex, Claude Code, and any future integration are install surfaces generated from the same package metadata and skill content.

## What It Provides

- **`interface-design`** - build or refactor product interfaces with intent, domain exploration, craft checks, visual direction, and design-system memory.
- **`interface-design-status`** - summarize the current `.interface-design/system.md`.
- **`interface-design-extract`** - infer a design system from existing frontend code and offer to write `.interface-design/system.md`.
- **`interface-design-audit`** - audit UI code against `.interface-design/system.md`.
- **`interface-design-critique`** - critique a built interface for generic/defaulted decisions and improve it.

Use this for product UI. Do not use it for landing pages, marketing sites, campaigns, or brand-only exploration.

## One Skill, Multiple Install Surfaces

Canonical sources:

- `install-surfaces/manifest.json` - shared package metadata.
- `skills/interface-design/` - canonical skill content.
- `skills/interface-design-*` - additional local entrypoints for status, extract, audit, and critique workflows.

Generated install surfaces:

- `.codex-plugin/plugin.json`
- `.agents/plugins/marketplace.json`
- `.claude/skills/interface-design/`
- `.claude/commands/`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`

Run this after changing canonical skill content or package metadata:

```bash
scripts/sync-install-surfaces
```

Use check mode in CI or before committing:

```bash
scripts/sync-install-surfaces --check
```

## Repository Layout

```text
.
├── install-surfaces/
│   └── manifest.json
├── .codex-plugin/
│   └── plugin.json
├── .agents/
│   └── plugins/
│       └── marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .claude/
│   ├── commands/
│   └── skills/
├── skills/
│   ├── interface-design/
│   │   ├── SKILL.md
│   │   ├── agents/openai.yaml
│   │   └── references/
│   ├── interface-design-audit/
│   ├── interface-design-critique/
│   ├── interface-design-extract/
│   └── interface-design-status/
├── reference/
│   ├── system-template.md
│   └── examples/
├── scripts/
│   └── sync-install-surfaces
└── AGENTS.md
```

## Install Surface: Codex

Clone the repository:

```bash
git clone https://github.com/Kirchberg/interface-design.git
cd interface-design
```

Codex can read the repo-local marketplace at `.agents/plugins/marketplace.json`. Restart Codex from this repository and install the `interface-design` plugin from the local marketplace.

For personal local testing, copy this repository into a personal plugin folder and point `~/.agents/plugins/marketplace.json` at it:

```bash
mkdir -p ~/.codex/plugins
cp -R "$(pwd)" ~/.codex/plugins/interface-design
mkdir -p ~/.agents/plugins
```

Then add a personal marketplace entry like:

```json
{
  "name": "personal-plugins",
  "interface": {
    "displayName": "Personal Plugins"
  },
  "plugins": [
    {
      "name": "interface-design",
      "source": {
        "source": "local",
        "path": "./.codex/plugins/interface-design"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Design"
    }
  ]
}
```

Restart Codex after changing marketplace or plugin files. Local plugins are installed into Codex's plugin cache, so edits to the source folder may require reinstalling or refreshing the cached copy.

## Install Surface: Claude Code

For plugin-style local testing, point Claude Code at this repository's `.claude-plugin/marketplace.json` or copy the repository into the plugin location you use for local Claude Code marketplaces.

For manual skill installation:

```bash
mkdir -p ~/.claude/skills
cp -R .claude/skills/interface-design ~/.claude/skills/
```

The generated command wrappers under `.claude/commands/` expose these slash-command entries:

```text
/interface-design:init
/interface-design:status
/interface-design:audit <path>
/interface-design:extract <path>
/interface-design:critique <path>
```

Do not edit generated install-surface files directly. Update `install-surfaces/manifest.json` or `skills/interface-design/`, then run `scripts/sync-install-surfaces`.

## Use

Ask naturally:

```text
Use Interface Design to build the analytics dashboard.
```

```text
Audit the settings UI against .interface-design/system.md.
```

```text
Extract a design system from src/components and write .interface-design/system.md.
```

When the current tool supports slash commands or skill mentions, these are equivalent:

```text
/interface-design
/interface-design status
/interface-design audit src/components
/interface-design extract
/interface-design critique
```

You can also explicitly mention skills by name, for example `$interface-design-audit`.

## How The Main Skill Works

When `interface-design` runs, the agent:

1. Checks for `.interface-design/system.md`.
2. Uses saved direction, tokens, depth, spacing, and component patterns when present.
3. If no system exists, establishes a product-specific direction from:
   - the human using the interface;
   - the task they must perform;
   - the intended feel;
   - domain concepts and color world;
   - one signature element;
   - defaults to reject.
4. Uses image generation when a direction board, UI reference, screenshot paintover, or project-bound raster asset would clarify the work.
5. Builds UI using token architecture, subtle layering, systematic spacing, clear typography, and explicit states.
6. Evaluates the result with swap, squint, signature, token, and state checks.
7. Offers to save reusable decisions to `.interface-design/system.md`.

## Visual Direction

The main skill can use image generation as a design companion when useful. Generated images are references, not implementation.

Good uses:

- direction boards before a greenfield screen;
- medium-fidelity UI references for major redesigns;
- screenshot paintovers when a working UI lacks craft;
- project-bound raster assets such as empty states, onboarding images, placeholders, or product previews.

The durable decisions from those references should become tokens, layout, components, and documented system memory.

## System Memory

`.interface-design/system.md` stores project-specific design decisions:

```markdown
# Design System

## Direction
Personality: ...
Foundation: ...
Depth: ...

## Tokens
### Spacing
Base: ...
Scale: ...

### Colors
...

## Patterns
### Button Primary
- Height: ...
- Padding: ...
- Radius: ...
- Usage: ...

## Decisions
| Decision | Rationale | Date |
```

Templates and examples live in `reference/`.

## Install Surface Notes

- Shared metadata lives in `install-surfaces/manifest.json`.
- Skill content lives under `skills/`.
- Tool-specific install surfaces are generated by `scripts/sync-install-surfaces`.
- `AGENTS.md` documents contribution expectations.
- This plugin does not bundle hooks or rules by default. The workflow is guidance-oriented; add hooks only for deterministic enforcement.
- Generated surfaces should be treated as build outputs checked into the repository for local installation.

## Maintenance

After changing canonical skill content, package metadata, or generated-surface rules:

```bash
scripts/sync-install-surfaces
scripts/sync-install-surfaces --check
```

The repository hook at `.githooks/pre-commit` runs check mode. Install it locally only if you want Git to enforce generated-surface sync before commits:

```bash
git config core.hooksPath .githooks
```

## Credits

Based on the original `interface-design` / `claude-design-skill` work by Damola Akinleye, adapted here for multi-surface agent usage.

## License

MIT. See [LICENSE](LICENSE).
