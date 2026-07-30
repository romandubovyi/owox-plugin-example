# Data Marts Cards

A small OWOX Data Marts plugin: it reads the project's data marts through the host
bridge and lays them out as a card grid — status, storage, source, triggers and reports
on each card — rather than the table the app itself shows.

It doubles as the reference plugin for exercising the gallery end to end, from
publication through to opening it in its sandboxed iframe.

## What this repository has to contain, and why

| Path | Why it exists |
| --- | --- |
| `plugin.json` | The manifest OWOX reads at the exact commit a release points at. |
| `.well-known/owox-plugin.json` | OWOX fetches this at publication time to confirm the URL really hosts a plugin. |
| `.nojekyll` | GitHub Pages runs Jekyll, which silently drops any path starting with a dot — including `.well-known`. |
| `index.html` | The page OWOX embeds. |

The page implements the handshake inline instead of importing `@owox/plugin-sdk`,
because that package is not published to npm yet and GitHub Pages runs no build step.
It speaks the same protocol, so the host bridge is exercised for real.

## Publishing it

1. Enable GitHub Pages for this repository, serving from `main`.
2. Cut a release tagged `v1.0.0`. Without a release there is no eligible version, and
   the plugin will publish with nothing to install.
3. Publish it in OWOX — from the web app at member or project scope, or with
   `owox-ctl plugins publish romandubovyi/owox-plugin-example --scope deployment --all-projects`.

The tag must be exactly `MAJOR.MINOR.PATCH`, optionally `v`-prefixed. Prerelease
identifiers and build metadata are refused: the highest eligible version becomes current
for everyone immediately and nobody can pin an older one.

## What it needs from OWOX

`GET /api/data-marts`, and nothing else. The plugin holds no credential: the host page
mints a short-lived runtime token, keeps it out of this frame entirely, and makes the
call itself — so the list is exactly what the member who installed the plugin can see.

Pagination is followed to the end, so a project with more data marts than one page still
shows all of them.

If the deployment cannot mint a runtime token, the page says so instead of rendering an
empty grid. A deployment-wide suspension is reported as its own message: the installation
is intact and returns when an administrator resumes the plugin.
