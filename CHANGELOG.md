# Changelog

All notable changes to the tikin plugin are documented here.

## Unreleased

- **Config-file fallback for the API key:** every skill's setup gate now resolves
  `TIKIN_API_KEY` from the environment first, and if it's unset, sources a dotenv file at
  `~/.config/tikin/env` (honors `XDG_CONFIG_HOME`; can also carry `TIKIN_BASE_URL`). The env
  var still takes precedence, so nothing changes for existing users.

## 0.1.0

Initial release.

- **Cross-agent by design:** skills follow the [Agent Skills](https://agentskills.io) open
  standard and work in Claude Code, Codex, and other skills-compatible agents. Install via
  `npx skills add`, the Claude Code plugin marketplace, or manual copy.
- **Foundation skills:** `tikin-onboarding`, `tikin-rest-api`, `tikin-endpoint-discovery`
  (searches 1,000+ endpoints via its bundled `scripts/tikin-find-endpoint` — self-contained,
  no PATH setup; the tool travels with the skill).
- **Platform skills:** `tiktok`, `douyin`, `instagram`, `youtube`, `twitter-threads`,
  `xiaohongshu`.
- **Task skills:** `social-media-downloader`, `creator-analytics`, `trend-research`,
  `social-listening`, `competitor-analysis`, `hashtag-research`, `comments-analysis`,
  `bulk-data-export`.
- Single REST path against `https://console.tikin.net` with a tikin API key; per-call prepaid
  billing; balance/usage via `GET /api/usage/token/`.
- Bundled endpoint index covering 1,000+ endpoints across the supported platforms.
