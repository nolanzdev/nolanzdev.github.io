---
title: "VideoLens: deconstruct viral short videos and reverse-engineer their prompts"
date: 2026-06-02 14:00:00 +0800
categories: [产品记录]
tags: [AI, short video, prompts, MCP, VideoLens]
lang: en
permalink: /posts/videolens/
canonical_url: https://blog.nolanzhao.xyz/post/videolens-short-video-deconstruction-20260602/
---

> Originally published on [云端漫步](https://blog.nolanzhao.xyz/post/videolens-short-video-deconstruction-20260602/). Reposted here.
{: .prompt-info }

VideoLens is a tool site I built for deconstructing viral short videos. Paste a Xiaohongshu / Douyin share link, or upload a local clip, and it takes the video apart for you — analyzing the audiovisual language shot by shot, and reverse-engineering the prompts you'd need to recreate it.

> 💡 Visit: <https://videolens.cc/en>
{: .prompt-tip }

## What you get

Submit a video and VideoLens returns two things:

**① A director-grade breakdown report**

- **Full transcript** (timestamped) — the spoken lines, accurate to the second
- **Shot-by-shot breakdown** — shot size, composition, transitions, and camera moves for every shot
- **Pacing and hooks** — the opening hook, rhythm beats, and information density
- **Visual analysis** — stitched keyframes for each shot plus a breakdown of what's in the frame

**② Reconstructed prompts**

- Prompts you can use to recreate the video, **in both English and Chinese**
- Ready to feed into video generation models like Sora, Kling, or Dreamina

## Who it's for

| Audience | How they use it |
| --- | --- |
| **Short-video creators / influencers** | Deconstruct the hooks and pacing of viral hits to produce similar content faster |
| **Content ops / MCN agencies** | Batch-analyze benchmark accounts and build a reusable topic & script playbook |
| **E-commerce sellers** | Extract the pain-point hooks and conversion logic of product videos and apply them to your own |
| **AI video creators** | Turn videos you love into generation prompts for Sora / Kling / Dreamina |
| **Directors / editors** | Study audiovisual language shot by shot (shot size, composition, transitions, beat-synced BGM) as storyboard reference |
| **Ad / marketing planners** | Research viral creative formulas and distill repeatable script frameworks |

## How to use it

### 1. On the web

The simplest way — open [videolens.cc/en](https://videolens.cc/en):

- Paste a Xiaohongshu / Douyin share link, or
- Upload a local video (mp4, ≤ 30MB / ≤ 5 minutes)

Processing runs asynchronously after you submit. When it's done, the results page walks through the breakdown shot by shot, and the "Copy" button in the top-right corner copies all the prompts in one click.

### 2. MCP (new)

VideoLens ships a local MCP server, so MCP-capable AI clients like Claude Desktop and Cursor can call its video-analysis capabilities directly. Two steps to connect:

**① Log in from the terminal** (paste the API key from the settings page; it's saved locally):

```bash
npx -y @videolens/mcp login
```

**② Add this to your AI client's MCP config** (no key needed here):

```json
{
  "mcpServers": {
    "videolens": {
      "command": "npx",
      "args": ["-y", "@videolens/mcp"]
    }
  }
}
```

Once connected, you can just talk to it in natural language:

- "Analyze this Xiaohongshu video for me: https://www.xiaohongshu.com/…"
- "Upload /tmp/demo.mp4 and analyze it"
- "Is my video abc123 done yet?"
- "How much analysis quota do I have left?"

The MCP server wraps 7 tools in total — `analyze_url`, `analyze_file`, `analyze_image`, `get_result`, `get_image_result`, `list_my_videos`, `get_quota` — covering the full flow of submitting an analysis, checking progress, and checking quota.

## Recent updates

- **Improved prompts** — reconstructed prompts now match the original video more closely
- **Upgraded the underlying model** — sharper visual understanding
- **Added MCP support** — call it directly from Claude Desktop / Cursor

Try it out: <https://videolens.cc/en>
