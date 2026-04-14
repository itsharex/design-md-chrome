# Chrome Extension Implementation Plan

Build a Manifest V3 Chrome extension that extracts UI style signals from the active page and generates either `DESIGN.md` or `SKILL.md`, using the local [DESIGN blueprint](./DESIGN.md) as the canonical output contract.

This workflow is based on TypeUI configuration: [https://typeui.sh/](https://typeui.sh/).  
Users can browse curated design skills here: [https://www.typeui.sh/design-skills](https://www.typeui.sh/design-skills).  
Chrome extension setup follows the official tutorial flow: [https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world](https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world).

## 1. Product Goals

- Extract meaningful style foundations from the current tab (typography, color, spacing, radius/shadow/motion).
- Infer reusable semantic tokens from raw CSS values.
- Generate:
  - `DESIGN.md` for design-system blueprint use.
  - `SKILL.md` for reusable agentic design skill files.
- Keep output implementation-ready, testable, and aligned with the quality gates in [DESIGN.md](./DESIGN.md).

## 2. Architecture (Manifest V3)

- `manifest.json`
  - `manifest_version: 3`
  - Permissions: `activeTab`, `scripting`, `storage`, `downloads`
  - Icons sourced from `logo.png` (resized to extension icon set)
- `service-worker.js`
  - Coordinates extraction runs and export actions.
- `content-script.js`
  - Reads DOM/computed styles from active page.
- `popup/`
  - UI controls for auto-generation, mode selection (`DESIGN.md` vs `SKILL.md`), preview, and download.
- `lib/`
  - `extract.js`: style collection
  - `normalize.js`: token inference and dedupe
  - `generate-design-md.js`: DESIGN.md formatter
  - `generate-skill-md.js`: SKILL.md formatter
  - `validate.js`: output conformance checks
- `assets/icons/`
  - Generated from root `logo.png`:
    - `icon16.png`
    - `icon32.png`
    - `icon48.png`
    - `icon128.png`
  - Used for both extension icons and action icon.

## 3. Extraction and Normalization Strategy

- Collect style samples from semantically relevant elements:
  - headings, paragraphs, links, buttons, inputs, cards, nav, table rows.
- Capture:
  - Typography: family, size, line-height, weight, letter spacing.
  - Color: text, background, border, focus outline.
  - Spacing: margin/padding distributions.
  - Shape/elevation: border radius, shadows.
  - Motion: transition/animation durations and easing.
- Normalize:
  - Group repeated values by frequency.
  - Round near-identical values into token buckets.
  - Produce semantic candidates (example: `color.text.primary`, `space.4`, `radius.md`).
- Store confidence score per inferred token to surface uncertain mappings in preview.

## 4. Markdown Generation Rules

## 4.1 `DESIGN.md`

- Must include all blueprint sections from [DESIGN.md](./DESIGN.md):
  - Mission
  - Brand
  - Style Foundations
  - Accessibility
  - Writing Tone
  - Rules: Do / Rules: Don’t
  - Guideline Authoring Workflow
  - Required Output Structure
  - Component Rule Expectations
  - Quality Gates
- Must preserve explicit rule language:
  - Non-negotiable constraints use `must`.
  - Recommendations use `should`.

## 4.2 `SKILL.md`

- Must include valid frontmatter:
  - `name: design-system-[brand-or-scope]`
  - `description: ...`
- Must include managed markers when requested:
  - `<!-- TYPEUI_SH_MANAGED_START -->`
  - `<!-- TYPEUI_SH_MANAGED_END -->`
- Must keep guidance concise and operational, with explicit component states and accessibility constraints.

## 5. UX Flow (Popup)

1. User opens popup and automatic extraction starts instantly.
2. User can switch output mode (`DESIGN.md` or `SKILL.md`).
3. User can click `Refresh` to re-run extraction after page changes.
4. Extension shows preview with:
  - inferred tokens
  - coverage summary
  - warnings for low-confidence mappings
5. User clicks `Download`.

## 6. Validation and QA

- Unit checks for:
  - token normalization stability
  - markdown section completeness
  - quality-gate wording (`must` vs `should`)
- Conformance checks:
  - required sections present
  - required states present (`default`, `hover`, `focus-visible`, `active`, `disabled`, `loading`, `error`)
  - accessibility constraints included and testable
- Manual test matrix:
  - marketing site
  - dashboard app
  - content-heavy page

## 7. Delivery Milestones

## Milestone 1: Extension Scaffold
- Create MV3 files, popup shell, and active-tab messaging.
- Set extension branding with `logo.png`-derived icons in `manifest.json`.
- Load unpacked extension and verify baseline UI.

## Milestone 2: Style Extraction
- Implement content-script sampling and payload transfer.
- Add debug preview of raw style groups.

## Milestone 3: Token Inference
- Implement normalization and semantic token naming.
- Add confidence scoring and dedupe.

## Milestone 4: Markdown Generators
- Implement `DESIGN.md` and `SKILL.md` generators.
- Enforce blueprint-required sections and wording gates.

## Milestone 5: Export + Validation
- Implement download/copy actions.
- Run unit + manual QA and fix edge cases.

## 8. Definition of Done

- Extension extracts styles from real pages and generates downloadable markdown.
- `DESIGN.md` output conforms to the local blueprint and acceptance checklist.
- `SKILL.md` output is reusable across repos with minimal variable replacement.
- Extension icon and toolbar icon are set from `logo.png`.
- Documentation includes TypeUI references and curated skills link.
