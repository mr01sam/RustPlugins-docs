# RustPlugins-docs

Public documentation site for mr01sam's Rust Oxide/Carbon plugins. Source code for the plugins themselves lives in separate, private repos — this repo only contains Markdown docs and is safe to make public or publish via GitHub Pages.

## Structure

- `docs/index.md` — landing page
- `docs/plugins/<plugin>.md` — one page per plugin (config reference, permissions, commands, behavior notes)
- `mkdocs.yml` — site config and nav

## Local preview

```bash
pip install -r requirements.txt
mkdocs serve
```

Then open http://127.0.0.1:8000

## Publishing

```bash
mkdocs gh-deploy
```

This builds the site and pushes it to the `gh-pages` branch, which GitHub Pages serves automatically (enable Pages for this repo once, pointing at the `gh-pages` branch).

## Adding a new plugin page

1. Create `docs/plugins/<plugin>.md`
2. Add it under `nav: Plugins:` in `mkdocs.yml`
3. Keep it accurate to the plugin's actual `Configuration`/permissions/commands — regenerate from source when the plugin's config changes.
