# tikin-agent-plugin — moved

**tikin-plugin now lives in [cookaihq/plugin-marketplace](https://github.com/cookaihq/plugin-marketplace),
in the [`tikin-plugin/`](https://github.com/cookaihq/plugin-marketplace/tree/main/tikin-plugin) directory.**
This repository is kept only as a forwarding marketplace so existing installs keep working.

## If you already installed from here

Nothing to do. This repository still publishes the `tikin-plugins` marketplace, and its
`tikin-plugin` entry now points at the new location, so the plugin content you receive comes from
`cookaihq/plugin-marketplace`. Refresh as usual:

```bash
claude plugin marketplace update tikin-plugins   # Claude Code
codex plugin marketplace upgrade tikin-plugins   # Codex
```

To move onto the new marketplace name at your convenience:

```bash
claude plugin marketplace add cookaihq/plugin-marketplace
claude plugin install tikin-plugin@plugin-marketplace
```

## If you are installing for the first time

Use the new marketplace directly.

**Claude Code**

```bash
claude plugin marketplace add cookaihq/plugin-marketplace
claude plugin install tikin-plugin@plugin-marketplace
```

**Codex**

```bash
codex plugin marketplace add cookaihq/plugin-marketplace
codex plugin add tikin-plugin@plugin-marketplace
```

**Any agent — skills CLI**

```bash
npx skills add https://github.com/cookaihq/plugin-marketplace/tree/main/tikin-plugin
```

## Where things went

| What | Where it is now |
|---|---|
| Skills, scripts, tests, changelog, contributing guide | [`plugin-marketplace/tikin-plugin/`](https://github.com/cookaihq/plugin-marketplace/tree/main/tikin-plugin) |
| Documentation | [`plugin-marketplace/tikin-plugin/README.md`](https://github.com/cookaihq/plugin-marketplace/tree/main/tikin-plugin/README.md) |
| Issues and pull requests | [cookaihq/plugin-marketplace](https://github.com/cookaihq/plugin-marketplace) |

The full commit history up to the move is preserved both here and inside
`cookaihq/plugin-marketplace`.

## Links

- Website <https://tikin.net> · Console <https://console.tikin.net>

## License

MIT — see [LICENSE](LICENSE).
