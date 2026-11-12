---
title: Concepts
description: Learn what a project, a plan and the hub are in AICW Video.
---

## Project

A project is a folder on your computer. It holds your source video, source audio and all files that AICW Video makes from them.

AICW Video keeps projects in `~/aicw-video/projects/`. Each project has its own folder there. The hub lists the folders in that location.

A project can hold more than one video, and separate audio tracks. When you add a video and an audio file, AICW Video can match the audio to the video. The `autoMatchAudio` option of the `create_project` MCP tool controls this. It is on by default.

AICW Video does not change your original files. It copies them into the project.

## Pipeline files

AICW Video writes a structured JSON file at each stage. Later stages read these files. A video folder can contain these files:

| File | Content |
| --- | --- |
| `audio.wav` | The audio that AICW Video extracted from the video. |
| `transcript.json`, `transcript.srt`, `transcript.words.json` | The speech text, with timing for each sentence and each word. |
| `analysis/moments.json` | Key moments that scene detection, frames and the transcript give. |
| `description.json` | A summary and a caption for each key frame. AI scene analysis makes this file. |
| `shorts/suggestions.json` | Clip candidates that a score ranks. |
| `shorts/plan.json` | The clip plan. |
| `shorts/render-<timestamp>/` | The rendered clips of one render run. |

The folder `shorts/latest` is a link to the newest render folder.

## Plan

The plan is the list of clips that you want. It is saved in `shorts/plan.json`. For each clip, the plan stores:

- an id, a title, and a start time and an end time in milliseconds;
- the caption lines;
- the caption style and the caption animation;
- the reframe mode, which is `crop` or `letterbox-blur`;
- the privacy options for faces and voice.

You can make a plan in four ways. You can ask the heuristic scorer with `suggest`. You can ask an AI with `plan-clips`. You can ask your AI agent to call `save_clip_plan`. You can edit the clips in the plan UI. A plan that AICW Video reads must pass its schema check.

If you edit `plan.json` by hand, keep the plan valid. AICW Video assumes that the user or the AI host wrote the plan.

## Hub

The hub is a small web page that runs on your computer. It listens on `127.0.0.1` only, so no other computer can reach it. The first port it tries is 8764. If that port is busy, it tries the next ports, up to 20 in all.

The hub has two tabs. **Plans** lists your projects and shows their stages: source, transcript, analysis, plan and renders. **How To Use** shows the setup for AI agents and the packaged Claude skill.

<Frame>
  <img src="/img/hub.png" alt="The AICW Video hub with two projects. Each project shows stage labels for source, transcript, analysis, plan and renders, and an Open plan UI button." />
</Frame>

## Plan UI

The plan UI is the page where you review one video. You open it from the hub with **Open plan UI**. It shows the source video, a timeline with key moments, and the clips. It saves your changes on every change. See [Typical workflow](/workflow).

<Frame>
  <img src="/img/plan-ui.png" alt="The plan UI for one video. It shows the source video, upload title and description fields, a timeline of clips, and a Save plan button." />
</Frame>
