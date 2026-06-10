---
title: "VideoLens：拆解爆款短视频，一键反推提示词"
date: 2026-06-02 14:00:00 +0800
categories: [产品记录]
tags: [AI, 短视频, 提示词, MCP, VideoLens]
lang: zh-CN
permalink: /posts/videolens/
canonical_url: https://blog.nolanzhao.xyz/post/videolens-short-video-deconstruction-20260602/
---

> 本文首发于 [云端漫步](https://blog.nolanzhao.xyz/post/videolens-short-video-deconstruction-20260602/)，此处为同步转载。
{: .prompt-info }

VideoLens 是我做的一个拆解爆款短视频的工具网站。贴一个小红书/抖音的分享链接，或者直接传一段本地短视频，它就能帮你把视频「拆开看」——逐镜分析视听语言，并反推出可以复刻这条视频的提示词。

> 💡 访问地址：<https://videolens.cc/zh>
{: .prompt-tip }

## 能得到什么

提交一条视频后，VideoLens 会输出两样东西：

**① 编导级拆解报告**

- **逐字稿**（带时间戳）——精确到秒的台词文本
- **分镜拆解**——逐镜的景别、构图、转场、运镜
- **节奏与钩子**——开头钩子、节奏卡点、信息密度
- **画面信息**——每个分镜的关键帧拼接图 + 画面要素分析

**② 还原的提示词**

- 可用于复刻这条视频的提示词，**含中英文对照**
- 可直接投喂给 Sora / 可灵 / 即梦 等视频生成模型

## 适合谁用 / 能干嘛

| 人群 | 用法 |
| --- | --- |
| **短视频创作者 / 博主** | 拆解爆款的结构钩子与节奏，快速产出同类内容 |
| **内容运营 / MCN** | 批量拆解对标账号，沉淀可复用的选题与脚本方法论 |
| **电商 / 带货商家** | 还原种草视频的痛点钩子与转化逻辑，套用到自己的产品 |
| **AI 视频创作者** | 把喜欢的视频还原成可投喂 Sora / 可灵 / 即梦 的生成提示词 |
| **编导 / 剪辑师** | 逐镜拆解视听语言（景别、构图、转场、BGM 卡点），做分镜参考 |
| **广告 / 营销策划** | 研究爆款创意公式，提炼可复制的脚本框架 |

## 使用方式

### 1. 网页端

最简单的方式，打开 [videolens.cc/zh](https://videolens.cc/zh) 即可：

- 粘贴小红书 / 抖音的分享链接，或
- 直接上传本地视频（mp4，≤ 30MB / ≤ 5 分钟）

提交后异步处理，处理完成后在结果页从上到下逐镜展示拆解内容，右上角「复制」按钮可一键复制全部提示词。

### 2. MCP（新）

VideoLens 提供了本地 MCP server，让 Claude Desktop、Cursor 等支持 MCP 协议的 AI 客户端直接调用视频分析能力。两步接入：

```bash
# ① 终端登录（粘贴从设置页获取的 API key，自动保存到本机）
npx -y @videolens/mcp login
```

```json
// ② 在 AI 客户端 MCP 配置里加上（无需再填密钥）
{
  "mcpServers": {
    "videolens": {
      "command": "npx",
      "args": ["-y", "@videolens/mcp"]
    }
  }
}
```

接入后可以直接用自然语言对话：

- “帮我分析这个小红书视频：https://www.xiaohongshu.com/…”
- “上传 /tmp/demo.mp4 并分析”
- “我的视频 abc123 分析完了吗？”
- “我还剩多少分析额度？”

MCP server 一共封装了 7 个工具：`analyze_url`、`analyze_file`、`analyze_image`、`get_result`、`get_image_result`、`list_my_videos`、`get_quota`，覆盖了提交分析、查询进度、查询额度的完整流程。

## 最近更新

- **优化了提示词** — 反推出来的提示词更贴合原视频
- **升级了大模型** — 画面理解更准
- **新增 MCP 功能** — 可在 Claude Desktop / Cursor 里直接调用

访问体验：<https://videolens.cc/zh>
