# AGENTS.md — slides-skills

Compact repo guide for OpenCode sessions. If a fact is obvious from the filenames, it is not here.

## What this repo is

- Source for the OpenCode skill `slides-skills` (also a Claude Code / Codex skill).
- It generates single-file, horizontal-swipe HTML slide decks in two visual systems:
  - **Style A** — editorial magazine × electronic ink.
  - **Style B** — Swiss International Typographic Style (strict grid, 22 locked layouts).
- License: AGPL-3.0.

## Directory reality check

- The actual skill payload is under **`skills/magazine-web-ppt/`**, not at the repo root.
- Root `README.md` / `README.en.md` / `CONTRIBUTING.md` still describe an older flat layout (`SKILL.md`, `assets/`, `scripts/`, `references/` at root). **Do not trust those paths literally.**
- Skill entry point: `skills/magazine-web-ppt/SKILL.md`.
- Skill assets:
  - Templates: `skills/magazine-web-ppt/assets/template.html` (Style A) and `template-swiss.html` (Style B).
  - Motion One fallback: `skills/magazine-web-ppt/assets/motion.min.js`.
  - Screenshot backgrounds: `skills/magazine-web-ppt/assets/screenshot-backgrounds/`.

## Build / test / lint

- None. This repo is static HTML/CSS/JS plus Markdown references.
- The only executable verification is the Swiss validator:

```bash
node skills/magazine-web-ppt/scripts/validate-swiss-deck.mjs <path-to-deck.html> [--allow-experimental]
```

- Note: `.github/pull_request_template.md` writes `node scripts/validate-swiss-deck.mjs ...`; from repo root use the full path shown above.
- PRs require manual visual QA (dense text slide, image slide, navigation/ESC/low-power mode) and the validator for Swiss decks.

## Editing the skill

- Templates are self-contained seed files. Each copy becomes a user deck.
- Before using any class from a layouts reference, verify it exists in the target template's `<style>` block. Missing classes are the #1 cause of broken decks.
- Style A and Style B classes are **not interchangeable**; do not mix them in one deck.
- When modifying templates:
  - **Style A**: add new classes to `assets/template.html` first; keep `references/layouts.md` in sync.
  - **Style B**: keep `assets/template-swiss.html`, `references/layouts-swiss.md`, `references/swiss-layout-lock.md`, and `scripts/validate-swiss-deck.mjs` in sync.
- New pitfalls should be added to `references/checklist.md` at the right P0/P1/P2/P3 tier.
- New theme colors go in `references/themes.md` (Style A) or `references/themes-swiss.md` (Style B) with a use-case note; custom hex values are not allowed by design.

## Swiss layout lock (Style B)

- Body slides must use one of the registered layouts `S01`–`S22` and declare it as `data-layout="Sxx"`.
- Only `SWISS-COVER-ASCII` and `SWISS-CLOSING-ASCII` are allowed outside the 22 body layouts.
- The validator catches unregistered layouts, missing `data-layout`, SVG text, centered body titles, and images missing `data-image-slot`.

## Key reference files

| File | Purpose |
|------|---------|
| `skills/magazine-web-ppt/SKILL.md` | Main skill workflow and principles |
| `skills/magazine-web-ppt/references/layouts.md` | Style A layout skeletons |
| `skills/magazine-web-ppt/references/layouts-swiss.md` | Style B layout skeletons |
| `skills/magazine-web-ppt/references/swiss-layout-lock.md` | Style B hard constraints and registered layouts |
| `skills/magazine-web-ppt/references/checklist.md` | Quality checklist (P0–P3) |
| `skills/magazine-web-ppt/references/components.md` | Component catalog (mostly Style A) |
| `skills/magazine-web-ppt/references/themes.md` / `themes-swiss.md` | Curated theme presets |
| `skills/magazine-web-ppt/references/image-prompts.md` | Image-generation prompts |
| `skills/magazine-web-ppt/references/screenshot-framing.md` | Screenshot framing rules |
| `skills/magazine-web-ppt/references/swiss-map-component.md` | S08 MapLibre map extension |

## Language

- Source skill docs are primarily Chinese; `README.en.md` is the English translation. Keep source docs in Chinese unless you are adding/updating the English counterpart.
