---
title: Troubleshooting
description: Fix common problems with AICW Video, and learn where to report a bug.
---

Start with the preflight check. It finds most problems.

```sh
aicw-video doctor
```

## doctor shows a missing whisper-cli

Install whisper.cpp with Homebrew.

```sh
brew install whisper-cpp
```

## doctor shows a missing ffmpeg or the subtitles filter

AICW Video needs an ffmpeg build with the libass (subtitles) filter. Install `ffmpeg-full` with Homebrew.

```sh
brew install ffmpeg-full
```

If you use another ffmpeg build, set `FFMPEG_PATH` to its path. See [Configuration](/configuration).

## The hub shows "error: Load failed" when you open a project

The first plan build can take a short time. AICW Video makes frame previews and caption style previews. Wait until the build ends, and then open the project again.

## Port 8764 is busy

The hub tries the next ports, up to 20 ports from 8764. Read the terminal output for the real address.

## The speech model is not found

The first transcription downloads the model `ggml-base.en.bin` to `~/.cache/aicw-video/`. This needs `curl` and a network connection. If you set `AICW_VIDEO_WHISPER_MODEL`, AICW Video does not download a model. It stops with an error if the file at that path does not exist. Correct the path or remove the variable.

## The plan command says there is no plan

The `plan` command and the `render` command need the file `shorts/plan.json`. Make a plan first. Use the plan UI, `plan-clips`, or the `save_clip_plan` MCP tool.

## Rendering is slow

If `doctor` says that `h264_videotoolbox` is missing, AICW Video uses the slower `libx264` encoder. Use an ffmpeg build that has `h264_videotoolbox`. More RAM helps with long videos and with many clips at the same time. You can lower the number of clips that render at the same time with `--concurrency` or `AICW_VIDEO_RENDER_CONCURRENCY`.

## The AI steps do nothing

If no AI tool is available, `go` skips the AI steps and uses the heuristic clip suggestions. Check the `ai_cli_tools` list in `config.json`. The `doctor` command tests each tool. See [Privacy and AI use](/privacy-and-ai-use).

If an AI host is connected through MCP, run the `ping_host` tool. It reports if the host supports sampling.

## The AI host does not show the AICW Video tools

- For Claude Desktop, quit the app with Cmd+Q and open it again.
- For Codex, restart it.
- For ChatGPT, use a remote MCP server. The local `aicw-video mcp` command does not work there.

See [Use with AI agents](/use-with-ai-agents).

## Caption preview and final video look different

The caption preview in the plan UI is approximate. The final ffmpeg render is the authority.

## Report a bug

If nothing helps, open an issue on [GitHub](https://github.com/aichatwatch/aicw-video/issues/new). For other contact, use the [quote page](https://aichatwatch.com/quote). To report a security problem, see [Contributing](/contributing#report-a-security-problem).
