# Immanuel, Decatur, Indiana, Church Records (Jupyter Book 2)

Jupyter Book 2 (MyST) version of the
[sphinx-immanuel](https://github.com/kurt-krueckeberg/sphinx-immanuel) Jupyter Book 1 project:
the Immanuel Lutheran Church register, Decatur, Indiana, 1850-1905.

All configuration, including the table of contents and the PDF export, is in `myst.yml`.

## Plugins

`myst.yml` loads two plugins from the repository root. They are the same files
as in [jb2-nla](https://github.com/kurt-krueckeberg/jb2-nla).

```yaml
project:
  plugins:
    - flat-table.mjs
    - link-images.mjs
```

### flat-table

Tables with cells spanning rows or columns use the `flat-table` directive from
JB1 (linuxdoc), written exactly as before with `{rspan}` and `{cspan}`. MyST has
no such directive, so `flat-table.mjs` provides it. The plugin turns each
flat-table into a native MyST table with the row and column spans set, so the
spans appear on the website and in the Typst PDF. No raw HTML tables are used.

JB2 ignores `:widths:` on a `list-table`, so a `list-table` with `:widths:`
is converted to a `flat-table`, which keeps the column widths. This project had
none.

### link-images

`link-images.mjs` makes every image a link to its own file, so clicking it opens
the full image in a new browser tab. It replaces the JB1 `clickable-images.js`.

## Requirements

- MyST, installed in a Python virtual environment:

  ```bash
  python3 -m venv .venv
  source .venv/bin/activate
  pip install mystmd
  ```

- Node.js 18 or later: `sudo dnf install nodejs`
- Typst, for the PDF: `sudo dnf install typst`

## Build

With the virtual environment active:

```bash
myst start          # serve the site as a MyST application (http://localhost:3000)
myst build --html   # static HTML in _build/html
myst build --pdf    # Typst PDF in exports/book.pdf
```

To view the static HTML locally:

```bash
python3 -m http.server 8000 -d _build/html   # then open http://localhost:8000
```

## Deploy to Cloudflare

The site is a Cloudflare Worker named `jb2-immanuel` that serves the static HTML in
`_build/html` (see `wrangler.jsonc`). Cloudflare builds it from this repository
on every push to `main`, with these settings (Worker → Settings → Build):

| Setting | Value |
|---|---|
| Build command | `pip install mystmd && myst build --html` |
| Deploy command | `npx wrangler deploy` |
| Root directory | `/` |
