<p align="center">
  <img src="assets/icon.svg" alt="Iconify Design Skills icon" width="128"/>
</p>

# Iconify Design Skills

[![CI](https://github.com/borg0ai/as-iconify/actions/workflows/ci.yml/badge.svg)](https://github.com/borg0ai/as-iconify/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![skills.sh](https://skills.sh/b/borg0ai/as-iconify)](https://skills.sh/borg0ai/as-iconify)

Agent Skills for designing distinctive app and OSS package icons that match product identity, from concept through platform assets.

Agent Skills for [Claude Code](https://code.claude.com/docs/en/skills) (and any
[Agent Skills](https://agentskills.io)-compatible tool). Each skill is a folder under
[`skills/`](skills/) with a single-purpose `SKILL.md`, trigger evals, and optional
scripts/references — validated on every write, commit, and PR.

## Quick start

Install with the [skills CLI](https://skills.sh) — auto-detects 70+ agents
(Claude Code, Codex, Cursor, Copilot, pi, …):

```bash
npx skills add borg0ai/as-iconify                  # all detected agents
npx skills add borg0ai/as-iconify -a codex -a pi   # or target specific agents
```

Or with the [GitHub CLI](https://cli.github.com/manual/gh_skill_install) (≥ 2.90),
including version-pinned installs from releases:

```bash
gh skill install borg0ai/as-iconify
gh skill install borg0ai/as-iconify <skill> --pin v0.3.0
```

Or as a Claude Code plugin:

```
/plugin marketplace add borg0ai/as-iconify
/plugin install iconify-design-skills@iconify-design-skills
```

Or copy a single skill into a project:

```bash
git clone https://github.com/borg0ai/as-iconify.git
cp -r as-iconify/skills/<skill-name> your-project/.claude/skills/
```

Then describe the task in Claude Code or invoke `/<skill-name>`. Use `/icon-design`
to route a complete project, or paste the end-to-end goal prompt from
[docs/setup-prompt.md](docs/setup-prompt.md).

## Skills

| Skill | Description |
| --- | --- |
| [icon-design](skills/icon-design/) | Guides the complete workflow, routes each stage to the right skill, and carries product identity, reference style, and palette through to export. |
| [icon-brief](skills/icon-brief/) | Derives icon concepts from product context and visual references, with distinctive silhouettes, style direction, palette roles, and small-size risks. |
| [icon-draw](skills/icon-draw/) | Draws 2-4 master 1024×1024 SVG candidates from an approved concept and palette, using the safe static SVG subset. |
| [icon-critique](skills/icon-critique/) | Renders candidates at 512/64/32/16 px, reviews the actual pixels, applies targeted fixes with a hard iteration cap, and waits for the human to pick. |
| [icon-export](skills/icon-export/) | Exports the approved master SVG to platform assets — favicon set, PWA/maskable, App Store/Play PNGs, macOS icns, GitHub social preview — with overwrite guards and size validation. |

## Repository structure

```
skills/                  # distributed skills, one folder per skill (SKILL.md + evals/ + scripts/)
docs/                    # skill-authoring guide, eval methodology, deployment guide
skills.sh.json           # skills.sh repo-page customization (groupings)
scripts/                 # deterministic validator used by hooks and CI
.claude/                 # agentic dev setup: hooks + bundled add-skill / publish-repo skills
.claude-plugin/          # plugin + marketplace manifests (makes this repo installable)
.local/                  # gitignored working area: sources, research, PROMPT.md (see below)
```

## Working on this repo with an agent

This repo is agent-native: canonical agent instructions live in
[AGENTS.md](AGENTS.md) (CLAUDE.md imports it), hooks validate every `SKILL.md` on
write, `make check` runs the full validator, and CI enforces the same gate on every
PR. The bundled `add-skill` skill walks the eval-first authoring workflow described
in [docs/skill-authoring.md](docs/skill-authoring.md). Maintainers drive sessions
with their own (gitignored, personal) `.local/PROMPT.md` goal prompt.

## Contributing

Contributions welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the skill-proposal
process, the authoring workflow, and the PR checklist. Please note the
[Code of Conduct](CODE_OF_CONDUCT.md).

## Support

Questions, ideas, or something not working? Start with [SUPPORT.md](SUPPORT.md) —
bugs and skill proposals have [issue templates](../../issues/new/choose), and
security concerns go through [SECURITY.md](SECURITY.md) (never a public issue).

## License

[MIT](LICENSE) © 2026 Paldom
