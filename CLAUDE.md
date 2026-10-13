# RustPlugins-docs

Public documentation site for mr01sam's Rust Oxide/Carbon plugins, built with MkDocs Material. Plugin source code lives in separate, private repos (e.g. `X:\RustPlugins\repos\SafePocket`) — only Markdown docs belong here.

## Adding a new plugin page

1. Read the plugin's actual source (`Configuration` class, permissions, commands) in its own repo — don't guess at config keys or defaults.
2. Create `docs/plugins/<plugin>.md` following the structure of `docs/plugins/safepocket.md`: permissions (note if they're dynamically generated from config, like Safe Pocket's tier overrides), a config reference table using the plugin's actual friendly JSON key names (from `[JsonProperty("...")]` attributes, not the raw C# field names), and a commands table.
3. Add the new page under `nav: Plugins:` in `mkdocs.yml`.

## Updating an existing page

When a plugin's config, permissions, or commands change, update its page to match — re-read the current source rather than assuming the doc is still accurate. Remove sections that no longer apply (e.g. a "Behavior notes" section should be dropped if it's no longer wanted, not left stale).

## Verifying before pushing

```bash
pip install -r requirements.txt   # first time only
python -m mkdocs build
```

Confirms the site builds without errors. `mkdocs serve` can be used instead for a live local preview at http://127.0.0.1:8000.

## Push and deploy

This repo deploys automatically — there is no manual `mkdocs gh-deploy` step needed day to day:

```bash
git add <files>
git commit -m "..."
git push
```

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site and pushes it to the `gh-pages` branch; GitHub Pages serves that branch directly (already configured: Settings → Pages → Deploy from branch → `gh-pages` → `/ (root)`). Check the repo's **Actions** tab if the live site doesn't update within a minute or two after a push — a 404 right after pushing usually just means the deploy is still in progress, not a real failure.

Live site: https://mr01sam.github.io/RustPlugins-docs/
