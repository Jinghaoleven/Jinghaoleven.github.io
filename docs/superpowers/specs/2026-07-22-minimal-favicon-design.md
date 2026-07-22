# Minimal Favicon Design

## Goal

Replace the current blue–orange `JZ` favicon with a simpler geometric mark that matches the homepage's warm ivory and academic green visual system.

## Approved design

- Keep the existing `64 × 64` SVG canvas and `assets/favicon.svg` path.
- Use a warm ivory rounded-square field with fill `#f4f2ec`.
- Add one thin academic-green border using `#1d5c4f`.
- Place one centered solid academic-green circle using `#1d5c4f`.
- Use no text, initials, gradients, shadows, nested frames, or decorative details.

The mark must remain recognizable at a browser-tab size of approximately `16 × 16` pixels and retain sufficient contrast on both light and dark browser chrome.

## Scope

Only `assets/favicon.svg` changes. The existing favicon link in `index.html`, page content, styling, metadata, and other assets remain unchanged.

## Validation

- Confirm the SVG is well-formed and uses the existing `0 0 64 64` view box.
- Confirm it contains no `<text>`, gradient, or shadow definitions.
- Confirm the only design colors are `#f4f2ec` and `#1d5c4f`.
- Render the SVG at `16 × 16` and inspect that the border and central dot remain distinct.
