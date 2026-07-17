---
name: tikin-rest-api
description: Call the tikin REST API directly with curl/HTTP. Covers base URL, Bearer auth, the /api/v1/{platform}/... path scheme, pagination, rate limits, retries, error handling, and per-call cost/balance awareness. Use for any direct data call against tikin.
---

# tikin — REST API

Direct HTTP access to all 1,000+ tikin endpoints. Use `tikin-endpoint-discovery` to find the
right path, then call it here.

## Setup gate

```bash
CONFIG="${XDG_CONFIG_HOME:-$HOME/.config}/tikin/env"
if [ -z "${TIKIN_API_KEY:-}" ] && [ -f "$CONFIG" ]; then set -a; . "$CONFIG"; set +a; fi
[ -z "${TIKIN_API_KEY:-}" ] && echo "Set TIKIN_API_KEY first (see tikin-onboarding)."
BASE="${TIKIN_BASE_URL:-https://console.tikin.net}"
```

## Essentials

- **Base URL:** `https://console.tikin.net` (override with `TIKIN_BASE_URL`).
- **Auth header:** `Authorization: Bearer $TIKIN_API_KEY`.
- **Path scheme:** `/api/v1/{platform}/{api}/{action}` — e.g. `/api/v1/tiktok/app/v3/fetch_one_video`.
- **Most reads are GET** with query params; batch/multi endpoints are POST with a JSON body.

## Action — example calls

```bash
BASE="${TIKIN_BASE_URL:-https://console.tikin.net}"

# TikTok: one video by id
curl -s "$BASE/api/v1/tiktok/app/v3/fetch_one_video?aweme_id=7372484719365098283" \
  -H "Authorization: Bearer $TIKIN_API_KEY"

# Instagram: user info
curl -s "$BASE/api/v1/instagram/v2/fetch_user_info?username=instagram" \
  -H "Authorization: Bearer $TIKIN_API_KEY"

# Douyin search (POST with a JSON body)
curl -s -X POST "$BASE/api/v1/douyin/search/fetch_general_search_v1" \
  -H "Authorization: Bearer $TIKIN_API_KEY" -H "Content-Type: application/json" \
  -d '{"keyword": "美食", "offset": 0, "count": 10}'

# Batch (POST) — some endpoints take a RAW JSON ARRAY body (not an object)
curl -s -X POST "$BASE/api/v1/tiktok/app/v3/fetch_multi_video" \
  -H "Authorization: Bearer $TIKIN_API_KEY" -H "Content-Type: application/json" \
  -d '["7372484719365098283","7372484719365098284"]'
```

Paths, methods, and parameters are exactly as returned by `tikin-find-endpoint` (the
`tikin-endpoint-discovery` skill's bundled search CLI) — pass them through unchanged.

## Pagination (param name varies by platform)

| Platform | Cursor param | Notes |
|---|---|---|
| TikTok / Douyin | `max_cursor` / `cursor` | response returns next cursor + `has_more` |
| Instagram | `pagination_token` | pass the token from the previous response |
| YouTube | `continuation_token` | pass to `..._replies` / next-page endpoints |
| Twitter | `cursor` | from previous timeline response |
| Threads | `end_cursor` | from previous response |
| Xiaohongshu | `cursor` (+ `index`) | |

Loop until the response's `has_more` is false or no next cursor is returned. **Each page is a
billed call — cap pages and warn the user before large pulls** (hand off to `bulk-data-export`
for big jobs).

## Rate limits, retries

- **QPS 10/sec.** Add a small delay or a concurrency cap (≤4) in loops.
- **Retry** transient 429/5xx up to 3× with exponential backoff (1s, 2s, 4s).

## Cost & balance awareness

tikin bills per call against your prepaid balance. Check balance/usage anytime:

```bash
curl -s "$BASE/api/usage/token/" -H "Authorization: Bearer $TIKIN_API_KEY"
```

Prices vary per endpoint. Cap pagination and estimate a run's cost (pages × per-call price)
before bulk pulls.

## Verification gate

Before claiming success:
1. HTTP 200 and body is valid JSON.
2. Not an auth/balance error (401 / insufficient balance).
3. Expected fields present (e.g. a video fetch returns a play/download URL).

## Red flags

- Hardcoding the API key in code/commits — always read `$TIKIN_API_KEY`.
- Unbounded pagination loops (runs up cost).
- Ignoring `has_more` / next-cursor and re-fetching page 1.
