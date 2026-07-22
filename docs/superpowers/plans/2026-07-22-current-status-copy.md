# Current Status Copy Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the repetitive hero research sentence with an approved two-line current-status block and update the About internship label without changing Experience.

**Architecture:** This is a static-site copy update contained in `index.html`, plus removal of the now-unused `.hero-summary` rules from `style.css`. Existing structure, behavior, visual styling, dates, and Experience content remain intact.

**Tech Stack:** HTML5, CSS3, inline JavaScript, shell-based static validation

## Global Constraints

- Hero line 1 must be exactly `Ph.D. Student · USTC & Shanghai Innovation Institute`.
- Hero line 2 must be exactly `TGT Program Research Intern · JD Explore Academy`.
- About sidebar internship must be exactly `TGT Program · JD Explore Academy`.
- The Experience section must remain unchanged.
- The existing research-interest list remains below the status block.
- No dependencies or new assets may be added.

---

### Task 1: Synchronize the current-status copy

**Files:**
- Modify: `index.html:55-91`
- Modify: `style.css:300-305, 881-883`
- Test: shell assertions against `index.html` and `style.css`

**Interfaces:**
- Consumes: the existing `.role` hero paragraph, About sidebar definition list, and `.research-tags` list.
- Produces: a two-line `.role` block and a synchronized About internship value; no new class or JavaScript interface.

- [ ] **Step 1: Run the pre-change assertion**

Run:

```bash
rg -F 'TGT Program Research Intern · JD Explore Academy' index.html
```

Expected: exit status `1` with no output because the approved hero line is not present yet.

- [ ] **Step 2: Update the hero and About copy**

Replace the current `.role` and `.hero-summary` blocks with:

```html
<p class="role">
  Ph.D. Student · USTC &amp; Shanghai Innovation Institute<br>
  TGT Program Research Intern · JD Explore Academy
</p>
```

Keep the existing `<ul class="research-tags">` immediately after this paragraph.

Replace the About internship value with:

```html
<dd>TGT Program · JD Explore Academy</dd>
```

Do not modify the Experience entry:

```html
<h3>Research Intern · JD Explore Academy</h3>
```

- [ ] **Step 3: Remove unused hero-summary styling**

Delete both unused `.hero-summary` rule blocks:

```css
.hero-summary {
  max-width: 720px;
  margin: 22px 0 0;
  font-size: 1rem;
}
```

and the mobile override:

```css
.hero-summary {
  margin-top: 22px;
}
```

- [ ] **Step 4: Run focused copy assertions**

Run:

```bash
rg -F 'Ph.D. Student · USTC &amp; Shanghai Innovation Institute<br>' index.html
rg -F 'TGT Program Research Intern · JD Explore Academy' index.html
rg -F '<dd>TGT Program · JD Explore Academy</dd>' index.html
rg -F '<h3>Research Intern · JD Explore Academy</h3>' index.html
! rg -F 'I study how foundation models learn after pre-training' index.html
! rg -F 'hero-summary' index.html style.css
```

Expected: the first four commands each print exactly one matching line; both negated searches produce no output and return success.

- [ ] **Step 5: Run static validation**

Run:

```bash
node -e 'const fs=require("fs"); const html=fs.readFileSync("index.html","utf8"); const refs=[...html.matchAll(/(?:src|href)="([^"]+)"/g)].map(m=>m[1]).filter(x=>!x.startsWith("http")&&!x.startsWith("mailto:")&&!x.startsWith("#")); const missing=refs.filter(x=>!fs.existsSync(x)); if(missing.length) throw new Error("Missing: "+missing.join(", ")); for(const m of html.matchAll(/<script>([\s\S]*?)<\/script>/g)) new Function(m[1]); console.log(`validated ${refs.length} local references and inline JavaScript`);'
node -e 'const css=require("fs").readFileSync("style.css","utf8"); const opens=(css.match(/\{/g)||[]).length; const closes=(css.match(/\}/g)||[]).length; if(opens!==closes) throw new Error(`Unbalanced CSS braces: ${opens}/${closes}`); console.log(`validated ${opens} CSS blocks`);'
git diff --check
```

Expected: both Node commands print validation success and `git diff --check` produces no output.

- [ ] **Step 6: Commit the implementation**

```bash
git add index.html style.css
git commit -m "content: update current academic status"
```
