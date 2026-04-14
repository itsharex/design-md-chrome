# TypeUI Style Extractor (Chrome Extension)

Chrome Extension (Manifest V3) that extracts style foundations from the active page and generates either:

- `DESIGN.md`
- `SKILL.md`

The generated output follows the local [DESIGN blueprint](./DESIGN.md).
Audience and product surface are inferred from live page signals (URL, metadata, headings, nav/CTA copy, and structure), not fixed defaults.

## References

- TypeUI configuration: [https://typeui.sh/](https://typeui.sh/)
- Curated design skills: [https://www.typeui.sh/design-skills](https://www.typeui.sh/design-skills)
- Chrome tutorial baseline: [https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world](https://developer.chrome.com/docs/extensions/get-started/tutorial/hello-world)

## Branding and Icons

This extension uses root `logo.png` as the source icon asset.

Generate required icon sizes:

```bash
./scripts/generate-icons.sh
```

Generated files:

- `assets/icons/icon16.png`
- `assets/icons/icon32.png`
- `assets/icons/icon48.png`
- `assets/icons/icon128.png`

## Local Setup

1. Open `chrome://extensions`
2. Enable **Developer mode**
3. Click **Load unpacked**
4. Select this project directory

## Usage

1. Open any website page.
2. Click the extension action.
3. The extension auto-generates markdown instantly from the active tab.
4. Optional: switch mode between `DESIGN.md` and `SKILL.md`.
5. Optional: click **Refresh** after page changes.
6. Review preview and validation warnings.
7. Click **Download** to save the generated markdown file.

## Project Structure

- `manifest.json` - MV3 configuration and icon wiring.
- `service-worker.js` - extraction orchestration and file export.
- `content-script.js` - DOM/computed style extraction.
- `popup/` - extension UI for mode switch, auto-generation, preview, and export.
- `lib/normalize.mjs` - style normalization and token inference.
- `lib/generate-design-md.mjs` - `DESIGN.md` generator.
- `lib/generate-skill-md.mjs` - `SKILL.md` generator.
- `lib/validate.mjs` - output conformance checks.
- `tests/run-tests.mjs` - smoke tests for generation/validation.

## Tests

Run:

```bash
node tests/run-tests.mjs
```

The tests validate:

- token inference from sample extraction payloads
- section completeness for `DESIGN.md` and `SKILL.md`
- required markers and accessibility constraints
