# Minimal Favicon Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the blue–orange `JZ` favicon with the approved warm-ivory and academic-green geometric mark.

**Architecture:** Keep the existing `assets/favicon.svg` path and `index.html` link. Replace only the SVG contents with one rounded rectangle and one centered circle, then validate the SVG structure and inspect a `16 × 16` raster rendering.

**Tech Stack:** SVG 1.1-compatible markup, xmllint, macOS sips

## Global Constraints

- The SVG canvas remains `viewBox="0 0 64 64"`.
- The only design colors are `#f4f2ec` and `#1d5c4f`.
- The asset contains no text, initials, gradients, shadows, nested frames, or decorative details.
- Only `assets/favicon.svg` may change during implementation.
- The existing favicon link and all page content remain unchanged.

---

### Task 1: Replace and validate the favicon

**Files:**
- Modify: `assets/favicon.svg:1-13`
- Test: shell assertions and a `16 × 16` raster rendering

**Interfaces:**
- Consumes: the existing `<link rel="icon" type="image/svg+xml" href="assets/favicon.svg">` in `index.html`.
- Produces: a valid SVG favicon at the same path; no new runtime interface or dependency.

- [ ] **Step 1: Run the pre-change assertion**

Run:

```bash
rg -F '<circle cx="32" cy="32" r="10" fill="#1d5c4f"/>' assets/favicon.svg
```

Expected: exit status `1` with no output because the approved central dot is not present yet.

- [ ] **Step 2: Replace the SVG with the minimal mark**

Set `assets/favicon.svg` to exactly:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect x="4" y="4" width="56" height="56" rx="14" fill="#f4f2ec" stroke="#1d5c4f" stroke-width="3"/>
  <circle cx="32" cy="32" r="10" fill="#1d5c4f"/>
</svg>
```

- [ ] **Step 3: Run structural assertions**

Run:

```bash
xmllint --noout assets/favicon.svg
rg -F 'viewBox="0 0 64 64"' assets/favicon.svg
rg -F '<rect x="4" y="4" width="56" height="56" rx="14" fill="#f4f2ec" stroke="#1d5c4f" stroke-width="3"/>' assets/favicon.svg
rg -F '<circle cx="32" cy="32" r="10" fill="#1d5c4f"/>' assets/favicon.svg
! rg '<text|linearGradient|radialGradient|filter|shadow' assets/favicon.svg
```

Expected: `xmllint` returns success, the three positive searches print one line each, and the negated search produces no output.

- [ ] **Step 4: Render and inspect the browser-tab size**

Run:

```bash
sips -s format png assets/favicon.svg --out /tmp/jinghao-favicon.png
sips -z 16 16 /tmp/jinghao-favicon.png --out /tmp/jinghao-favicon-16.png
sips -g pixelWidth -g pixelHeight /tmp/jinghao-favicon-16.png
```

Expected: the final image reports `pixelWidth: 16` and `pixelHeight: 16`. Inspect `/tmp/jinghao-favicon-16.png` and confirm the green border and central dot remain distinct on the ivory field.

- [ ] **Step 5: Verify scope and commit**

Run:

```bash
git diff --check
git diff --name-only HEAD
```

Expected: `git diff --check` produces no output and the implementation diff contains only `assets/favicon.svg`.

Commit:

```bash
git add assets/favicon.svg
git commit -m "design: simplify favicon"
```
