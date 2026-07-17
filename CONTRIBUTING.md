# Contributing to the tikin Plugin

Thanks for helping improve the tikin plugin for Claude Code! This plugin is a thin connector over
[tikin](https://tikin.net)'s REST API — most contributions are new/updated **skills** (Markdown)
and small helper scripts.

## Project layout

```
.claude-plugin/         plugin.json + marketplace.json (manifests — required)
skills/<name>/SKILL.md   one folder per skill
bin/tikin-find-endpoint  endpoint search CLI
skills/tikin-endpoint-discovery/references/endpoint-index.json  bundled index
```

## Quick start for contributors

```bash
git clone <your-fork-url> && cd tikin-plugin
export TIKIN_API_KEY="your_key"          # only needed to call the API, not to edit skills
claude --plugin-dir .                    # load the plugin locally to test
```

## Adding or editing a skill

- One folder per skill under `skills/`, each with a `SKILL.md` that opens with YAML frontmatter
  (`name`, `description`). Keep skills short and action-oriented.
- Every skill assumes a single REST base (`$BASE`, default `https://console.tikin.net`) and a
  `TIKIN_API_KEY` bearer token. Do not hardcode keys.
- Reference endpoints by their exact path/method/params — find them with `tikin-find-endpoint`.

## Endpoint index

The bundled index at `skills/tikin-endpoint-discovery/references/endpoint-index.json` ships with
the plugin and is refreshed by the maintainers on new releases when the API surface changes.

## License

By contributing, you agree your contributions are licensed under the MIT License.
