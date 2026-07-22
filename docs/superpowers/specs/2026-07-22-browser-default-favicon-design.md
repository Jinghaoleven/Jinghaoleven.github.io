# Browser-Default Favicon Design

## Goal

Keep the browser tab visually neutral by allowing each browser to display its own default webpage icon instead of a custom personal mark.

## Design Decision

Remove the explicit SVG favicon declaration from the document head:

```html
<link rel="icon" type="image/svg+xml" href="assets/favicon.svg">
```

Do not add a replacement favicon declaration. Browsers may render different default webpage icons; that variation is intentional and preferable to maintaining another custom symbol.

## Scope

- Change only `index.html` during implementation.
- Keep `assets/favicon.svg` in the repository as an unused historical asset.
- Do not change the page title, theme color, metadata, layout, colors, or content.
- Do not add PNG, ICO, emoji, lettermark, globe, or other custom favicon assets.

## Verification

- Confirm `index.html` contains no `rel="icon"` declaration.
- Confirm the remaining document head and page content are unchanged.
- Confirm the HTML still loads without a missing referenced favicon asset.
- Note that an already-open browser tab may retain the previous favicon until its cache is refreshed or the tab is reopened.
