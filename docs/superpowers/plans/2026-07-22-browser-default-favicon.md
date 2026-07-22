# Browser-Default Favicon Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove the site's explicit favicon declaration so browsers display their own default webpage icon.

**Architecture:** Make one isolated HTML-head change: remove the favicon `<link>` from `index.html` without replacing it. Keep the existing SVG asset untouched and verify that no other page metadata or files change.

**Tech Stack:** Static HTML, SVG asset, Git

## Global Constraints

- Change only `index.html` during implementation.
- Keep `assets/favicon.svg` in the repository as an unused historical asset.
- Do not change the page title, theme color, metadata, layout, colors, or content.
- Do not add PNG, ICO, emoji, lettermark, globe, or other custom favicon assets.

---

### Task 1: Use the Browser-Default Page Icon

**Files:**
- Modify: `index.html:22`
- Preserve unchanged: `assets/favicon.svg`

**Interfaces:**
- Consumes: the browser's native behavior when a document contains no favicon declaration.
- Produces: an HTML document head with no `rel="icon"` element.

- [ ] **Step 1: Run the failing assertion**

```bash
! rg -n '<link[^>]+rel="icon"' index.html
```

Expected: exit status 1 because the current file still contains the favicon declaration at line 22.

- [ ] **Step 2: Remove the explicit favicon declaration**

Delete this exact line from `index.html`:

```html
  <link rel="icon" type="image/svg+xml" href="assets/favicon.svg">
```

Do not add a replacement `<link>` element.

- [ ] **Step 3: Run the passing assertions**

```bash
! rg -n '<link[^>]+rel="icon"' index.html
rg -n '<meta name="theme-color" content="#f4f2ec">' index.html
rg -n '<title>Jinghao Zhang · 张景皓</title>' index.html
test -f assets/favicon.svg
```

Expected: exit status 0; the favicon declaration is absent, the title and theme color remain present, and the existing SVG asset remains in the repository.

- [ ] **Step 4: Verify the implementation scope**

```bash
git diff --check
test "$(git diff --name-only HEAD)" = "index.html"
git diff -- index.html
```

Expected: exit status 0; only `index.html` is modified, and its diff contains exactly one deleted favicon line.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "design: use browser-default favicon"
```
