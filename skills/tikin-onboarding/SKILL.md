---
name: tikin-onboarding
description: Entry point for using tikin social-media data in an AI agent. Use when the user first mentions tikin, asks to download/fetch TikTok/Douyin/Instagram/YouTube/Twitter/Xiaohongshu data, or hasn't set up access yet. Walks through getting an API key, setting TIKIN_API_KEY, and routes to the right skill.
---

# tikin — Onboarding & Routing

tikin is a unified API for social-media data across 16+ platforms (TikTok, Douyin,
Instagram, YouTube, Twitter/X, Threads, Xiaohongshu, Weibo, Bilibili, Reddit, LinkedIn,
and more): download media, fetch posts/profiles/comments, search, trends, and analytics.

Read this once, get the user set up, then **hand off to the skill that owns the task.**

## Step 1 — Get an API key (one time)

Send the user to <https://console.tikin.net>: register, open the API Keys page, and create a
key. Billing is per-call against a prepaid balance.

## Step 2 — Set the key in the environment

Every call reads `TIKIN_API_KEY` from the environment:

```bash
export TIKIN_API_KEY="your_api_key_here"
# persist it: add the line to ~/.zshrc or ~/.bashrc
# optional — override the base URL for a private deployment:
# export TIKIN_BASE_URL="https://console.tikin.net"
```

## Step 3 — Pick the path and hand off

tikin is a single REST API. Point the task at the right skill:

| If the user wants… | Hand off to |
|---|---|
| To find which endpoint does X (1,000+ exist) | `tikin-endpoint-discovery` |
| REST details (auth, paths, pagination, cost) | `tikin-rest-api` |
| A finished outcome (download, analyze a creator, trends, listening…) | `social-media-downloader`, `creator-analytics`, `trend-research`, `social-listening`, `competitor-analysis`, `hashtag-research`, `comments-analysis`, `bulk-data-export` |
| Deep work on one platform | `tiktok`, `douyin`, `instagram`, `youtube`, `twitter-threads`, `xiaohongshu` |

## Setup gate (run before any tikin call)

```bash
if [ -z "${TIKIN_API_KEY:-}" ]; then
  echo "TIKIN_API_KEY is not set — get a key at https://console.tikin.net then: export TIKIN_API_KEY=..."
fi
```

If unset, halt and walk the user through Steps 1–2 before proceeding.

## Red flags

- Calling endpoints before `TIKIN_API_KEY` is set (every call 401s).
- Forgetting that calls cost money — warn before large/bulk pulls.
