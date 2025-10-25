## Quick orientation

This repository is a static Bootstrap-based admin UI template (HTML/CSS/JS). There is no build system or server-side framework inside this folder — pages are plain HTML files in the project root and all assets live under `assets/`.

Keep these high-impact facts in mind when making changes:

- Entry and examples: `index.html` is the main/demo page. Many other pages (e.g. `productlist.html`, `addproduct.html`, `saleslist.html`) follow the same structure.
- Global styles & behavior: `assets/css/style.css` and `assets/js/script.js` contain the bulk of project-specific CSS and JS. Third‑party libraries live under `assets/plugins/`.
- Asset layout: `assets/{css,js,img,plugins}`. Plugins (FontAwesome, ApexCharts, DataTables, etc.) are referenced with relative paths inside HTML pages.

## Big-picture architecture and patterns

- Flat HTML site: each page is a full HTML document (header, sidebar, footer duplicated across files). There is no templating in this repo; expect repeated markup when editing pages.
- Client-side wiring: pages rely on a fixed script load order — jQuery -> plugin scripts (DataTables, ApexCharts, etc.) -> `bootstrap.bundle.min.js` -> `assets/js/script.js`. Changing that order will break initialisation.
- DataTables and charts: tables with the `datatable` class and charts are initialised by plugin-specific JS under `assets/js/` and `assets/plugins/` (e.g. `assets/plugins/apexchart/chart-data.js`). Look there first when debugging charts/tables.

## Developer workflows (how to preview, debug, and integrate)

- Preview locally: open `index.html` in a browser. For a better dev loop use VS Code Live Server or run a simple static server in PowerShell from the repo root:

  ```powershell
  cd "d:\Template HTML&CSS\tay-ninh-quan-template-admin"
  python -m http.server 8000
  # then open http://localhost:8000/index.html
  ```

- Alternative (if Node is available): `npx http-server -p 8000`.
- Debugging: use browser devtools. Common errors come from wrong relative asset paths or wrong script order (missing jQuery before plugins).

## Conventions & repository-specific patterns

- File naming: most pages are lowercase HTML filenames (sometimes hyphenated, sometimes not — e.g. `add-sales.html` and `addproduct.html`). Don't rely on consistent casing; reference exact filenames.
- Relative asset paths: pages use relative links like `assets/css/style.css`. If you serve pages from a subpath, switch to root‑relative paths (`/assets/...`) or update your server to serve the repo root.
- Re-use patterns: to add a new page, copy an existing page (for example `productlist.html`) and update the content; then add the page to the sidebar menu in the header/sidebar block.

## Integration notes (how to convert into a server-side app)

If you plan to integrate this template into a backend (Laravel, Express, etc.):

- Split shared markup into layout/partials (header, sidebar, footer). For Laravel Blade, move assets into the `public/assets` folder and replace static links with `{{ asset('assets/css/style.css') }}`.
- Maintain script order in your layout (jQuery first, then plugins, then app script).
- Example Blade head snippet:

  ```blade
  <link rel="stylesheet" href="{{ asset('assets/css/bootstrap.min.css') }}">
  <link rel="stylesheet" href="{{ asset('assets/css/style.css') }}">
  ```

## Key files to inspect when implementing features

- `index.html` — full example of header, sidebar, widget layout and script ordering.
- `assets/js/script.js` — global UI behaviour and component initialization.
- `assets/plugins/apexchart/chart-data.js` — chart setup used by the dashboard.
- `assets/css/style.css` — primary styling overrides for layout and components.
- `assets/plugins/*` — third party libs (DataTables, FontAwesome, ApexCharts, etc.).

## Common pitfalls to avoid

- Changing script/CSS order. Plugins depend on jQuery and on bootstrap being present when initialising.
- Using incorrect relative paths when serving from a different base path.
- Assuming the repo contains a build/test pipeline — it doesn't. Add a build tool only if you plan to modernize the stack.

## When to ask for help / missing info

- If you want CI, test, or a packaging/build pipeline added, tell us which tool you prefer (npm/Vite, Gulp, Laravel Mix) and whether you want assets transpiled/minified.

If anything in this guide is unclear or you want additional examples (Blade/Handlebars partials, a tiny npm build, or a Live Server workflow), tell me which integration you want first and I will update this doc.
