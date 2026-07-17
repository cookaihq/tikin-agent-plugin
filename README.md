# tikin Plugin

Social-media data for AI coding agents — download media and fetch posts, profiles, comments,
search, trends, and analytics across **TikTok, Douyin, Instagram, YouTube, Twitter/X, Threads,
Xiaohongshu**, and more, powered by [tikin](https://tikin.net).

Built on the [Agent Skills](https://agentskills.io) open standard — works in **Claude Code**,
**Codex**, and any skills-compatible agent. 17 skills across three layers: integration (REST),
per-platform coverage, and ready-made task workflows.

## Quick start

1. **Get an API key** at <https://console.tikin.net>: register, open the API Keys page, create a key.
2. **Provide it** — either export the env var, or drop it in a config file (env var wins):
   ```bash
   export TIKIN_API_KEY="your_api_key_here"
   # or persist it once for every skill:
   #   mkdir -p ~/.config/tikin && echo 'TIKIN_API_KEY=your_api_key_here' > ~/.config/tikin/env
   ```
   Skills read `TIKIN_API_KEY` from the environment; if it's unset they fall back to the
   `~/.config/tikin/env` dotenv file (honors `XDG_CONFIG_HOME`).
3. **Install** the skills for your agent — see [Install](#install) below.
4. Ask your agent something like *"Download this TikTok video"* or *"Analyze @nasa on Instagram"*.

## Install

The skills are self-contained [Agent Skills](https://agentskills.io) folders (the endpoint-search
tool ships inside the `tikin-endpoint-discovery` skill), so any compatible agent can use them.
Runtime requirements are just `curl` (REST calls) and `python3` (the bundled endpoint-search
script) — no SDK, package install, or build step.

### Any agent — skills CLI

[`npx skills`](https://github.com/vercel-labs/skills) installs into every agent it detects
(Claude Code, Codex, Cursor, and more):

```bash
npx skills add cookaihq/tikin-agent-plugin
```

### Claude Code — plugin marketplace

```bash
claude plugin marketplace add cookaihq/tikin-agent-plugin
claude plugin install tikin-plugin@tikin-plugins
```

Or test a local checkout: `claude --plugin-dir /path/to/tikin-plugin`.

### Codex — manual copy

Codex reads skills from `.agents/skills/` (project-level) or `~/.agents/skills/` (personal):

```bash
git clone https://github.com/cookaihq/tikin-agent-plugin.git
mkdir -p ~/.agents/skills
cp -R tikin-agent-plugin/skills/* ~/.agents/skills/
```

### Verify & update

- **skills CLI installs:** check with `npx skills list`, update with `npx skills update`.
- **Claude Code marketplace installs:** manage via the `/plugin` interface inside Claude Code.
- **Manual copies:** `git pull` in your clone, then re-copy `skills/*`.

Sanity check in any agent: ask *"Find the tikin endpoint for one TikTok video"* — the
`tikin-endpoint-discovery` skill should search the bundled index and return
`GET /api/v1/tiktok/app/v3/fetch_one_video`.

## How it connects

tikin exposes a single **REST API** at `https://console.tikin.net`. You call it with your tikin
API key; tikin authenticates, meters usage (per-call, prepaid balance), and returns the data. One
key, one base URL, standard HTTP — no SDK or extra runtime required.

| Path | Owned by skill |
|------|----------------|
| REST best practices (auth, paths, pagination, cost) | `tikin-rest-api` |
| Find the right endpoint among 1,000+ | `tikin-endpoint-discovery` |

## Skills

### Foundation
| Skill | What it does |
|-------|--------------|
| `tikin-onboarding` | Entry point: get a key, set `TIKIN_API_KEY`, route to the right skill |
| `tikin-rest-api` | REST best practices: auth, paths, pagination, rate limits, cost |
| `tikin-endpoint-discovery` | Search 1,000+ endpoints with `tikin-find-endpoint` |

### Platforms
| Skill | Coverage |
|-------|----------|
| `tiktok` | Videos, users, search, ads/trends, creator analytics, shop |
| `douyin` | Videos, users, search series, comments |
| `instagram` | Users, posts, search, comments, hashtags (V2) |
| `youtube` | Video info, streams, captions, comments, search (Web-V2) |
| `twitter-threads` | Tweets/posts, profiles, timelines, search, trends |
| `xiaohongshu` | Note details, users, search, comments (App-V2) |

### Tasks
| Skill | Outcome |
|-------|---------|
| `social-media-downloader` | Download video/audio/images (no-watermark) from any URL |
| `creator-analytics` | Profile + engagement + cadence + top content for an account |
| `trend-research` | Trending content, rising hashtags, hot sounds, ranking boards |
| `social-listening` | Mentions → sentiment → themes → cited digest |
| `competitor-analysis` | Benchmark multiple accounts side by side |
| `hashtag-research` | Hashtag volume, top content, related tags |
| `comments-analysis` | Comment sentiment, themes, top comments for a post |
| `bulk-data-export` | Paginate large lists → dedup → CSV/JSON, with cost estimate |

## Examples

```text
You: Download https://www.tiktok.com/@nasa/video/7372484719365098283 without watermark
→ social-media-downloader parses the URL and saves the no-watermark MP4.

You: How is @nasa doing on Instagram?
→ creator-analytics pulls the profile + recent posts and reports engagement rate & top content.

You: What's trending on TikTok in the US right now?
→ trend-research pulls popular trends, hashtags, and hot sounds for the US.
```

## Notes

- tikin bills per API call against your prepaid balance. Task skills warn before large pulls and
  cap pagination. Check your balance/usage anytime:
  ```bash
  curl "https://console.tikin.net/api/usage/token/" -H "Authorization: Bearer $TIKIN_API_KEY"
  ```
- Override the base URL with `TIKIN_BASE_URL` if you use a private deployment.

## Links

- Website <https://tikin.net> · Console <https://console.tikin.net> ·
  GitHub <https://github.com/cookaihq/tikin-agent-plugin>
- [Changelog](CHANGELOG.md) · [Contributing](CONTRIBUTING.md)

## License

MIT — see [LICENSE](LICENSE).
