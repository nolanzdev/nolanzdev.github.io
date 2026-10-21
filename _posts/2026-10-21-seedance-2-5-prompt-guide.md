---
title: "How to Write Seedance 2.5 Prompts: Differences from 2.0, Timestamped Shots and Reference Mapping"
date: 2026-10-21 12:00:00 +0800
categories: [创作方法论]
tags: [Seedance, AI video, prompting, VideoLens]
lang: en
permalink: /posts/seedance-2-5-prompt-guide/
canonical_url: https://videolens.cc/en/blog/seedance-2-5-prompt-guide
---

> Originally published on the VideoLens blog: [How to Write Seedance 2.5 Prompts: Differences from 2.0, Timestamped Shots and Reference Mapping](https://videolens.cc/en/blog/seedance-2-5-prompt-guide). Reposted here in full.
{: .prompt-info }

Short answer: write a Seedance 2.5 prompt in four parts — reference mapping → one-line summary → a timestamped (or shot-by-shot) breakdown → global notes. Compared with 2.0, 2.5 raises the clip length limit from 15 to 30 seconds and the reference limit from 15 to 50 items, and it responds to whole-second timestamps — so a prompt can grow from "a few shot descriptions" into "a timed shot script." Both 2.0 and 2.5 remain available side by side. Based on Volcengine Ark's official documentation, this guide first lays out how the two differ, then covers how to write for 2.5 and the common pitfalls, ending with a 30-second template you can adapt.

## I. How Seedance 2.5 differs from 2.0

The official summary of Seedance 2.5 is "long storytelling × strong references × precise editing × multilingual." Every spec below comes from Volcengine Ark's official Seedance 2.5 and Seedance 2.0 series tutorials:

| Item | Seedance 2.0 | Seedance 2.5 |
| --- | --- | --- |
| Clip length | 4–15s, or let the model choose | 4–30s, or let the model choose |
| Reference limit | 15: 9 images + 3 videos + 3 audio clips; 15s of audio/video in total | 50: 30 images + 10 videos + 10 audio clips; videos and audio each ≤30s in total |
| Audio-only reference | Not supported — must be paired with an image or video | Supported |
| Timing control | Organize by "Shot 1 / 2 / 3"; the official guide says exact timings (e.g. 0–3s) are unreliable | Responds to whole-second timestamps |
| Multi-view subject references | Not recommended | Supported |
| Output resolution | 480p / 720p / 1080p (8-bit), plus 4K (10-bit) | 480p / 720p / 1080p (10-bit, H.265); no 4K |
| Output aspect ratio | 6 fixed options: 21:9, 16:9, 4:3, 1:1, 3:4, 9:16 | The same 6 options; with adaptive, input material can yield any ratio from 0.4 to 2.5 |
| Output format | MP4 | MP4 or MOV (better color and A/V consistency for edits and extensions) |
| Edit / extend video | Supported | Supported; edits can target a time range by timestamp, and edit/extend/first-last-frame tasks lock the output aspect ratio (edits also lock duration) |
| Draft mode | Not supported | Supported: preview at 480p first, then render the final video |
| Languages | Not listed separately in the official tutorial | Officially lists 11 languages for prompts and audio: Chinese, English, Spanish, Indonesian, Malay, Thai, Arabic, Portuguese, Vietnamese, Japanese, Korean |

Which to use: 2.0 when you need 4K output; 2.5 when you need coherent clips longer than 15 seconds, more than 15 references, or per-second pacing.

> Heads-up: the official docs note that 2.5 and 2.0 have quite different aesthetic preferences — the same references and prompt look noticeably different across versions. To carry a 2.0 look into 2.5, the official advice is to feed the 2.0 output into a 2.5 "extend" task with MOV output.
{: .prompt-tip }

## II. The official four-part structure

The official framing: treat Seedance 2.5 as a visual content producer and write a structured prompt the way a director would. A complete prompt has four parts, in order:

| Part | What goes in it | Example |
| --- | --- | --- |
| ① Reference mapping | Number every image/video/audio in upload order and state its job (appearance, voice, motion, location). Skip for pure text-to-video | Image 1 is the lead, Lin Xia, voiced by Audio 1; use the orbit camera move from Video 1 |
| ② One-line summary | Subject + place + event + genre/style + any signature camera move | Realistic nature-documentary style: on a forest slope on a warm afternoon, a panda cub rolls down the hill |
| ③ Detailed beats | Split by timestamps or "Shot N"; for each segment write picture, camera, action, dialogue and SFX, phrased positively | 0s–3s: …; 3s–8s: … |
| ④ Global notes | Details that hold across the clip — camera, environment, sound, mood | Low angle, slight handheld feel; natural ambience; warm, real, natural overall |

Describe the picture positively; the negative phrasing the docs support is for subtitles and audio — e.g. "no subtitles", "no BGM".

## III. Writing timestamps

On 2.0 the official approach is "Shot 1 / Shot 2", with the model pacing each shot from the story; 2.5 responds to timestamps in one-second units, so a 30-second story can be paced second by second. Three forms are supported:

- Ranges: "0–3s … 3–7s … 7–15s" or "[1s-4s] … [4s-8s] … [8s-12s]"
- Points: "at 5s, a fast whip-pan left as the transition"
- Relative: "he stands frozen; 3 seconds later the people around him shake their heads", "after the shutter clicks, the frame holds for 1 second."

Official cautions:

1. Keep the timeline continuous — avoid gaps like "0–3s … 5–6s"
2. Match content to duration — too little and the model improvises, too much and it over-cuts or drops beats
3. Do not use timestamps to control frequency, e.g. "shake head 3 times per second"
4. Either timestamps or "Shot N" are fine for splitting the beats.

> One data point: when VideoLens split a 159-second sample into 30-second blocks, its 49 shots fell into 6 blocks, and the first 28-second block held 10 shots.
{: .prompt-tip }

## IV. Reference mapping: the more references, the clearer "who is who" must be

The official mapping rules:

1. Number in upload order (Image 1 / Video 1 / Audio 1) and bind every reference once in text; use a list when there are many people
2. Do not rely on names written on the image — labeling a portrait "Zhang San" and then writing "Zhang San at school…" invites mix-ups or duplicated characters
3. Be specific about what is borrowed, and which part for partial references: "the spell-casting motion from Video 1, the orbiting camera from Video 2", "the lighting and filter of Image 1"
4. When a reference is already precise, just point to it instead of re-describing: "strictly follow the motion and camera of Video 1, in the same order"
5. With multiple characters, upload references in order of first appearance and renumber the prompt to match — this avoids character swaps.

| Reference type | Official recommendation |
| --- | --- |
| Subject images | 1–8 subjects work well; 9–12 is possible but less stable and may need rerolls |
| Subject audio/video | 1–5 subjects, 5–10s each, work well |
| Multi-view | Up to 5 subjects, single or multi-view both work; beyond 5, single-view is more stable — split multi-view sheets into separate images |
| Storyboard grids | Best under 15 panels; prefer stick-figure or line art without heavy text; grids are not followed strictly — use multiple keyframe references when you need exact framing |
| Blockout (white-model) reference | Coarse blockouts built from simple shapes work best |

> Per the official docs, Seedance 2.5 does not accept reference images/videos containing real human faces directly; portrait work goes through the platform's portrait-asset workflow.
{: .prompt-tip }

## V. Camera language, action and expression

| Rule | How to write it |
| --- | --- |
| Use common terms directly | Shot size (extreme wide/wide/medium/close/close-up), moves (push/pull/pan/truck/follow/orbit/dive/pull-back/tilt-up/handheld shake), angles (low, top-down, first-person) |
| Popular moves work as-is | One-take, dolly zoom, aerial view, FPV, bullet time, handheld, rebound speed ramp |
| Explain niche terms | Write "term + description", e.g. "rack focus: focus shifts smoothly, the foreground trees blur while the person behind comes into focus" |
| Spell out transitions | "At 5s, fast whip-pan left (wipe left + natural dissolve)" |
| Summarize action | Prefer summaries like "does several sets of high knees and flips"; detail only the memorable moments and never repeat the same action |
| Describe expressions plainly | Avoid idioms: instead of "eating with relish", write "a satisfied smile, taking big bites" |
| Keep strong emotion out of the eyes | Words like "fanatical" or "utterly shocked" can make pupils glow; use milder words and state "normal human eyes, not glowing" |

## VI. Dialogue, subtitles and sound

The official 2.5 tutorial keeps the same sound symbols as 2.0: music in ( ), sound effects in < >, dialogue in { }, subtitles in 【】. For non-Chinese dialogue, state the language before the line. Because 【】 means subtitles and ( ) means music, avoid those brackets for section labels or tone notes inside the prompt.

Negative control: for subtitles write "no subtitles" or "do not add dialogue captions"; audio can be controlled separately for SFX, BGM and dialogue — e.g. "no BGM, only ambience and action sounds", "no sound at all."

Still getting subtitles after "no subtitles"? The docs say this is much improved over 2.0 but cannot be fully avoided; to reduce it:

1. Do not repeat words after a line and tack on tone, expression or action descriptions
2. When a line needs emotion, split it as "Character line (emotion): content"
3. Make sure your reference video has no burned-in subtitles.

When on-screen text must be exact: split English words into separate letters and ask for them to appear in sequence; for Chinese, render the correct copy as an image, supply it as a reference, and cite that image number after the text description.

## VII. Writing edit and extend prompts

2.5 infers the task type from the prompt, so edits and extensions need trigger words:

- Edit: include one of "edit video, add, delete/remove, modify/replace/change to" — e.g. "add some small animals to @video1", "change the person in @video1 to @image1", "remove the background music from @video1"
- Extend: include one of "extend forward/backward, continue" — e.g. "extend @video1 backward; the character from @image1 drops from the sky…".

For edits, state the scope and the change, optionally limited by timestamp: "in Video 1, change the man drinking coffee at 4–6s to mopping the floor; keep everything else unchanged." Official tips: source videos under 20 seconds edit best (they must be 4–30s), and using MOV for both input and output keeps color and audio continuity better when editing or extending.

## VIII. A 30-second template you can adapt

Below is a text-to-video example in the four-part structure (pure text, so no reference mapping): a one-line summary, timestamped beats, then global notes. Dialogue, SFX and music are marked with { }, < > and ( ). Swap the subject, setting and lines to reuse it:

**30-second prompt template**

```text
Realistic and cinematic: on a rainy night at a city-corner convenience store, a young woman just off work gets the last skewer of oden and shares a small, warm moment with the clerk.

0–4s: Extreme wide, locked-off. A rainy street corner; the store's warm yellow light spills across the wet pavement. The woman walks in from frame left under a clear umbrella, closes it and pushes the door open, <door chime>.
4–9s: Medium, following. She reaches the oden counter, sees only one daikon skewer left in the pot, and lets out a small sigh of relief.
9–14s: Close, over-the-shoulder. A middle-aged male clerk puts the daikon in a paper cup, adds an extra ladle of broth and says gently: {Rainy day — have some more soup.}
14–20s: Close-up. She cups the paper cup in both hands; steam fogs her glasses and she can't help laughing.
20–26s: Medium, slow push-in. She sits on a stool by the window sipping the broth; the rain eases outside and headlights streak across the glass. She says softly: {Good job today, too.}
26–30s: Extreme wide, slow pull-out. The camera retreats outside; the store glows like a lamp on the rainy corner as the frame fades down.

Warm yellow vs. cool blue lighting, shallow depth of field, slight handheld feel. <steady rain>, <simmering oden>, (soft, low-volume piano music). Realistic body proportions and physics. No subtitles, no watermark, no logo.
```

For reference-to-video, put reference mapping first — e.g. "Image 1 is the woman, Image 2 is the clerk, Image 3 is the store interior; Image 1 uses the voice from Audio 1" — and trim the character descriptions in the body accordingly.

## IX. Troubleshooting at a glance

| Symptom | Official fix |
| --- | --- |
| Subtitles despite "no subtitles" | Do not repeat words + tone after lines; use "Character line (emotion): content"; use subtitle-free reference videos |
| Characters swapped in multi-person scenes | Upload references in order of first appearance and renumber |
| Glowing eyes | Add "normal human eyes, not glowing" and soften extreme emotion words |
| Misspelled on-screen text | Spell English letter by letter in sequence; supply Chinese copy as an image |
| Fingerprint-like texture on grass/leaves | Keep reference images no larger than the output resolution; 1080p output shows it less often than 720p |
| Unstable results from overloaded tasks | Split into several simple generate/edit steps and assemble in post |
| Style does not match your 2.0 clips | Feed the 2.0 clip into an extend task with MOV output |

## X. FAQ

Q: Can I reuse Seedance 2.0 prompts on 2.5?  
A: Structurally, yes — "Shot N" splitting and the ( ) < > { } 【】 sound symbols carry over. But the two versions have different aesthetic preferences, so the same prompt will not look the same; to use 2.5's longer length, swap shot numbers for timestamps and re-pace for 30 seconds.

Q: Should I write prompts in Chinese or English?  
A: Either. The docs list 11 supported languages for prompts and audio on 2.5; for non-Chinese dialogue, state the language before the line.

Q: What about videos longer than 30 seconds?  
A: 2.5 supports multiple rounds of extension: use keywords like "extend backward" or "continue" with the generated video as input. Or split the story into segments of 30 seconds or less, generate each, and cut them together.

## XI. Generate shot prompts from a reference video with VideoLens

Writing a 30-second prompt by hand means laying out every second. VideoLens runs it in reverse: give it a reference video (upload, or paste a Douyin/Xiaohongshu link) and it breaks the clip down shot by shot, outputting shot prompts with anchored characters/locations/props, per-shot camera and action, dialogue and SFX. Both Seedance 2.0 and 2.5 are supported — pick one before uploading: 2.5 splits into 30-second blocks with up to 6 keyframes each, 2.0 into 15-second blocks.

> Want a ready-to-tweak shot prompt? [Go to Video to Prompt](https://videolens.cc/en/video-to-prompt)
{: .prompt-tip }

> Specs and techniques here are compiled from Volcengine Ark's official "Doubao Seedance 2.5 Prompt Guide", "Doubao Seedance 2.5 Tutorial" and the Seedance 2.0 series tutorial and prompt guide (retrieved 2026-10-21); the latest official docs take precedence. The example prompt was written by us following the official structure. For 2.0, see our Seedance 2.0 prompt guide. [Read the 2.0 guide](https://videolens.cc/en/blog/seedance-2-prompt-guide)
{: .prompt-tip }

A Seedance 2.5 prompt is essentially a timed shot script: state what each reference is for, set the tone in one line, write each time segment's picture, camera, action and sound, then add the constraints that hold across the clip. Your 2.0 habits still work on 2.5, so pick the version that suits the job — and if you would rather not start from scratch, give VideoLens a reference video and it returns a shot prompt split to the chosen version's length limit, ready to tweak.
