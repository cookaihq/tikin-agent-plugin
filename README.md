# tikin-agent-plugin — legacy marketplace

**New releases are published as `tikin-social` in
[cookaihq/aihub-marketplace](https://github.com/cookaihq/aihub-marketplace).**
The same repository is available on
[CNB](https://cnb.cool/zhidateam/tannt/aihub-marketplace).

This repository retains the `tikin-plugins` marketplace and the `tikin-plugin`
Plugin name for existing installations. Both marketplace manifests now point to
the last release under that name, **0.3.0**, using the tag
`tikin-plugin/v0.3.0` and commit
`7ca01b514d1bcf4bf26761bfacfbebeef94a83bc` in `cookaihq/plugin-marketplace`.
The source stays available even after the old directory is removed from that
repository's `main` branch. Refreshing this legacy marketplace keeps you on
0.3.0; it does not rename the installed Plugin or deliver `tikin-social` updates.

## Move an existing installation

1. Add `aihub-marketplace` and install `tikin-social` using the installation request below.
2. Prepare the new Plugin's configuration. Version 0.3.0 reads
   `~/.config/tikin/.env` and `~/.config/tikin/settings.json`, or
   `$XDG_CONFIG_HOME/tikin/` when that variable is set. If you want to reuse
   those files, ask the Agent to identify the exact source and destination,
   compare existing settings without displaying secrets, and wait for your
   explicit migration choice before writing. The new destinations are
   `~/.config/tikin-social/.env` and `~/.config/tikin-social/settings.json`;
   the new Plugin does not use `XDG_CONFIG_HOME` to locate them. Check `.env`
   and the routing preferences in `settings.json` separately, preserve the
   source files, and resolve destination conflicts before copying. Nothing is
   moved or read from the old directory automatically. API key fields and the
   17 `tikin-*` Skill names are unchanged; ordinary Skill configuration in
   `~/.config/<skill-name>/.env` remains in place.
3. Disable `tikin-plugin` in your host's Plugin manager, so the two copies of
   the same Skills are not active together. This also applies if you installed
   `tikin-plugin@plugin-marketplace`.
4. Start a new session. Check the new Plugin identity, its version, and the
   actual source of its Skills before verifying configuration and use. The
   active Plugin must be `tikin-social@aihub-marketplace`, and the Skills must
   come from that installation.
5. Once those checks succeed, you can uninstall the old Plugin through the
   host's Plugin manager. Keep the original configuration until you explicitly
   decide to remove it.

Installing the new Plugin does not move credentials or replace your existing
installation. See the [current configuration and migration instructions](https://github.com/cookaihq/aihub-marketplace/blob/main/tikin-social/README.md)
for the complete configuration order and verification steps.

## Install from the new marketplace

Copy this request to your Agent in Claude Code, Codex, or WorkBuddy:

```text
Install the complete tikin-social Plugin from AIhub Marketplace. Use https://github.com/cookaihq/aihub-marketplace, or https://cnb.cool/zhidateam/tannt/aihub-marketplace.git if GitHub has a network failure. Read the marketplace and Plugin README first, then use the current host's native Plugin manager. In WorkBuddy, use its native suite manager and show the bundled entry screenshot when I need to operate the UI; do not start a separate CodeBuddy CLI. Preserve my configuration and report Plugin installation, Skill discovery, and actual-use verification separately.
```

The [marketplace guide](https://github.com/cookaihq/aihub-marketplace#readme)
also lists `aihub-studio`. The current tikin README retains the separate Agent
Skills installation option and explains its scope. Check the Plugin README's
runtime support before installation; a marketplace entry alone does not prove
native Windows helper compatibility.

## Repository locations

| What | Location |
|---|---|
| Current Plugin, Skills, scripts, tests and changelog | [`aihub-marketplace/tikin-social/`](https://github.com/cookaihq/aihub-marketplace/tree/main/tikin-social) |
| Current documentation | [`tikin-social/README.md`](https://github.com/cookaihq/aihub-marketplace/blob/main/tikin-social/README.md) |
| New issues and pull requests | [cookaihq/aihub-marketplace](https://github.com/cookaihq/aihub-marketplace) |
| Fixed legacy 0.3.0 source | [`plugin-marketplace/tikin-plugin/` at the release commit](https://github.com/cookaihq/plugin-marketplace/tree/7ca01b514d1bcf4bf26761bfacfbebeef94a83bc/tikin-plugin) |

The original tikin history remains in this repository. The Plugin's history,
including the later changes made in `plugin-marketplace`, is preserved in
`aihub-marketplace` through Git subtree history extraction and import.

## Links

- Website <https://tikin.net> · Console <https://console.tikin.net>

## License

MIT — see [LICENSE](LICENSE).
