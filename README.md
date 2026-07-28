# OWOX example plugin

A deliberately minimal plugin for exercising the OWOX Data Marts plugin gallery from
publication through to opening it in its sandboxed iframe.

It shows the context the host hands over, and offers a button that calls the OWOX API
through the host bridge.

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

## Known limitation while the runtime authorization endpoint is pending

The handshake completes and the context arrives, because neither needs a credential.
The **Call the OWOX API** button fails, because the host cannot mint a runtime token yet.
That failure is the expected state, not a defect in this plugin.
