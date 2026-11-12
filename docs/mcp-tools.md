---
title: MCP tools
description: The 15 tools that the AICW Video MCP server gives to an AI host.
---

The MCP server runs on standard input and output. Start it with `aicw-video mcp`. An AI host such as Claude Code starts it for you. See [Use with AI agents](/use-with-ai-agents).

In the tools, `project` is a project slug from `list_projects` or the absolute path of a project folder. If a project has one video, you can leave out `video`. Otherwise, `video` is the slug of the video.

## Projects

### list_projects

Lists the projects in `~/aicw-video/projects/`. It takes no arguments.

### get_project

Reads one project.

- `project`: the project slug or path.

### create_project

Creates a project from local video and audio files. The files are copied into the `_sources` folder of the project. Unsupported files and files that are not found are skipped, and the result lists them.

- `files`: a list of file paths. Required.
- `name`: the project title. If you leave it out, AICW Video makes one from the files.
- `autoMatchAudio`: match external audio to video. The default is `true`.
- `aiSceneAnalysis`: turn on AI scene analysis. The default is `false`.

## Analysis

### describe_project

Describes all videos and audio files of a project. In MCP mode, frame analysis and caption proofreading use the model of the host, not a local AI tool.

- `project`: required.
- `visualContext`: text that tells the AI what the video shows.
- `force`: run again. The default is `false`.
- `aiSceneAnalysis`: the default is `true`.

### prepare_clips

Creates a folder for each video, matches external audio, makes clip suggestions and builds the plan UI of each video.

- `project`: required.
- `forceMatchAudio`: the default is `false`.
- `forceDescribe`: the default is `false`.

### analyze_project

Runs the normal pipeline. It describes the sources and then prepares the clips.

- `project`: required.
- `visualContext`: text that tells the AI what the video shows.
- `forceDescribe`: the default is `false`.
- `forceMatchAudio`: the default is `false`.
- `aiSceneAnalysis`: the default is `true`.

## Plans and review

### get_clip_plan

Reads `shorts/plan.json` of a video.

- `project`: required.
- `video`: optional.

### save_clip_plan

Checks a plan against the schema and saves it as `shorts/plan.json`.

- `project`: required.
- `video`: optional.
- `plan`: the plan, with its list of clips. Required.

### build_plan_ui

Builds the plan UI page for the current plan of a video.

- `project`: required.
- `video`: optional.

### review_project

Starts the hub, or reuses it, and returns local review links for the plan UI of each video.

- `project`: required.
- `video`: optional.
- `port`: the port to use.
- `openBrowser`: open the browser on macOS. The default is `false`.
- `includePreviewImages`: add preview images to the answer. The default is `false`.
- `maxPreviewImages`: from 1 to 6. The default is 3.

## Render and export

### render_clip

Renders one clip.

- `project`: required.
- `video`: optional.
- `clipId`: the clip id. Required.
- `aspects`: a list of formats. The values are `9:16`, `youtube-shorts`, `1:1`, `4:5`, `linkedin` and `16:9`. If you leave it out, the tool renders the clip once, without a format list.

### render_all_clips

Renders every clip in the plan of a video.

- `project`: required.
- `video`: optional.
- `concurrency`: from 1 to 12.

### export_clip_tutorial

Exports one clip as a tutorial folder. It uses the captioned moments and frames of the clip.

- `project`: required.
- `video`: optional.
- `clipId`: required.
- `title`: the tutorial title. The default is the title of the clip.
- `steps`: from 1 to 30.
- `format`: `html`, `md` or `both`. The default is `both`.

## App and host

### open_app

Starts the hub, or reuses it, and returns its local URL and port.

- `port`: the port to use.
- `openBrowser`: open the browser on macOS. The default is `false`.

### ping_host

Tests if the host supports MCP sampling. With sampling, the server can ask the host model for text. If the host does not support it, the tool reports `sampling unavailable`.

- `phrase`: the text to send. The default is `hello from AICW Video`.
- `includeImage`: also test an image. The default is `false`.
