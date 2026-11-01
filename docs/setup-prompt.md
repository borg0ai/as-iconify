# Setup prompt: run Iconify Design Skills against a repo

One paste-ready `/goal` that lets `icon-design` route brief → draw → critique → export. Product identity, user references, and approved palette stay consistent across stages. Export reads the approved `icon-design/icon.svg` and stops if it is missing.

## Prerequisites

- In the target repo's Claude Code session: `/plugin marketplace add borg0ai/as-iconify` then `/plugin install iconify-design-skills@iconify-design-skills` (or copy the `skills/*` folders into the target's `.claude/skills/`).
- `python3` (all pipeline scripts are stdlib-only) and an SVG renderer — `rsvg-convert`, `resvg`, `cairosvg`, Inkscape, or ImageMagick; macOS's built-in `qlmanage` is a zero-install fallback.

## The prompt

```text
/goal Design and ship this repository's icon with Iconify Design Skills, keeping
every artifact under icon-design/ until the final wiring step. Match product
purpose, audience, supplied visual references, and brand palette. Do not impose
a dark-grey background, symmetry, or a three-color cap when the design says
otherwise. Up to seven purposeful colors are supported; test legibility at
small sizes. Report unsupported SVG effects before changing the design.

1. DESIGN — use `icon-design` to inspect existing artifacts and route the next
   focused skill. `icon-brief` must capture product context, distinct concepts,
   visual direction, palette roles, and small-size risks.
2. DRAW — use `icon-draw` for 2-4 candidates that follow the approved brief.
   Run its SVG checker; fix errors and explain any remaining warnings.
3. CRITIQUE — use `icon-critique` to lint and render every candidate at
   512/64/32/16 px. Score from pixels and show every candidate. Stop for my pick;
   never auto-promote, including unattended runs. On approval, save the winner
   to `icon-design/icon.svg`.
4. EXPORT — only after approval, use `icon-export` on `icon-design/icon.svg`.
   Wire assets into the repo only if I asked. Existing files require explicit
   approval before overwrite. Report the exporter's WROTE table and warnings.
5. HANDOFF — leave all changes in the working tree. Report the chosen concept,
   palette, validation results, and changed files for review.

Definition of Done: approved design brief; ≥2 valid candidates; critique table
and renders; human-approved `icon-design/icon.svg`; export validated when
requested; nothing committed; blockers reported honestly.
```

## Notes

- The "exit 0" gates are real: `check_svg.py`, `render_icon.py`, and `export_icons.py` all exit non-zero on failure; the exporter refuses to overwrite without `--force` (exit 3) and prints a `WROTE <file> (WxH)` line per validated asset.
- Skill routing lives in `icon-design/SKILL.md`; invoke a focused skill directly for single-stage requests.
- Parallel agents never share a file: one candidate file per draw agent, one `renders/<candidate>/` dir per critique agent.
