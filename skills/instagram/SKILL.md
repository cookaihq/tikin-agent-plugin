---
name: instagram
description: Work with Instagram data via tikin — fetch user info and posts, search users/reels/hashtags/music/locations, pull post comments and replies, and hashtag feeds. Use when the task targets Instagram. Covers the Instagram V2 API.
---

# Instagram (via tikin)

Deep coverage of Instagram via the V2 API. Exhaustive endpoints via the
`tikin-endpoint-discovery` skill: `tikin-find-endpoint "<goal>" --platform instagram`.

## Setup gate

```bash
CONFIG="${XDG_CONFIG_HOME:-$HOME/.config}/tikin/env"
if [ -z "${TIKIN_API_KEY:-}" ] && [ -f "$CONFIG" ]; then set -a; . "$CONFIG"; set +a; fi
[ -z "${TIKIN_API_KEY:-}" ] && echo "Set TIKIN_API_KEY first (see tikin-onboarding)."
BASE="${TIKIN_BASE_URL:-https://console.tikin.net}"
```

**Coverage:** Instagram-V2. (V1 and V3 are de-scoped — reach them via discovery.)

## Key endpoints

| Goal | Method + path | Key params |
|---|---|---|
| User info | `GET /api/v1/instagram/v2/fetch_user_info` | `username` \| `user_id` |
| User posts | `GET /api/v1/instagram/v2/fetch_user_posts` | `username` \| `user_id`, `pagination_token` |
| User reels | `GET /api/v1/instagram/v2/fetch_user_reels` | `username` \| `user_id`, `pagination_token` |
| User stories | `GET /api/v1/instagram/v2/fetch_user_stories` | `username` \| `user_id` |
| Post info | `GET /api/v1/instagram/v2/fetch_post_info` | `code_or_url` |
| Search users | `GET /api/v1/instagram/v2/search_users` | `keyword` |
| General search | `GET /api/v1/instagram/v2/general_search` | `keyword`, `pagination_token` |
| Search reels | `GET /api/v1/instagram/v2/search_reels` | `keyword`, `pagination_token` |
| Search hashtags | `GET /api/v1/instagram/v2/search_hashtags` | `keyword` |
| Hashtag posts | `GET /api/v1/instagram/v2/fetch_hashtag_posts` | `keyword`, `feed_type`, `pagination_token` |
| Post comments | `GET /api/v1/instagram/v2/fetch_post_comments` | `code_or_url`, `sort_by`, `pagination_token` |
| Comment replies | `GET /api/v1/instagram/v2/fetch_comment_replies` | `code_or_url`, `comment_id`, `pagination_token` |
| Shortcode → media id | `GET /api/v1/instagram/v2/shortcode_to_media_id` | `shortcode` |

## Example

```bash
curl -s "$BASE/api/v1/instagram/v2/fetch_user_posts?username=instagram" \
  -H "Authorization: Bearer $TIKIN_API_KEY"
```

## Pagination

Everything uses `pagination_token` — pass the token returned by the previous response; stop when
none is returned. **Each page is billed — cap it.**

## Hand off to task skills

- Download posts/reels → `social-media-downloader`
- Analyze an account → `creator-analytics`
- Hashtag work → `hashtag-research`
- Comment mining → `comments-analysis`
- Listening across keywords → `social-listening`
- Large pulls → `bulk-data-export`

## Red flags

- Post-level endpoints take `code_or_url` (the shortcode or full post URL), not the numeric id —
  convert with `shortcode_to_media_id` if needed.
