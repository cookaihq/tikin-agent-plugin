# tikin Plugin for Claude Code

Social-media data for Claude — download media and fetch posts, profiles, comments, search,
trends, and analytics across **TikTok, Douyin, Instagram, YouTube, Twitter/X, Threads,
Xiaohongshu**, and more, powered by [tikin](https://tikin.net).

17 skills across three layers: integration (REST), per-platform coverage, and ready-made task
workflows.

## Quick start

1. **Get an API key** at <https://console.tikin.net>: register, open the API Keys page, create a key.
2. **Export it:**
   ```bash
   export TIKIN_API_KEY="your_api_key_here"
   ```
3. **Install** from the marketplace, or test locally:
   ```bash
   claude --plugin-dir /path/to/tikin-plugin
   ```
4. Ask Claude something like *"Download this TikTok video"* or *"Analyze @nasa on Instagram"*.

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

- Website <https://tikin.net> · Console <https://console.tikin.net>

## License

MIT — see [LICENSE](LICENSE).
