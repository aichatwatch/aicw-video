---
title: Requirements
description: The system and tools that AICW Video needs, and how to check them with doctor.
---

## What you need

| Requirement | Notes |
| --- | --- |
| macOS | AICW Video supports macOS today. Windows support is planned. |
| 8 GB RAM or more | More RAM helps with long source videos and parallel renders. |
| Node.js 20 or later | Runs the command line tool, the MCP server and the hub. |
| `ffmpeg-full` and `ffprobe` | Extract audio, sample frames, read video information and render clips. Caption rendering needs the libass (subtitles) filter of ffmpeg. |
| `whisper-cpp` | Transcribes speech on your computer. It provides the `whisper-cli` command. |
| `curl` | Downloads the speech model the first time. |
| `tensorflow` | AICW Video installs it as a library for face detection on your computer. |

An AI tool is optional. You need one only when you turn on AI scene analysis. The options are Claude Code, Claude Desktop, Codex CLI, a ChatGPT-compatible remote MCP host or Ollama. Claude Code is the recommended and best tested path today. See [Privacy and AI use](/privacy-and-ai-use).

Voice-over is alpha. It works on macOS only, and it uses the system text-to-speech engine.

## Check your setup with doctor

The `doctor` command checks your computer. It reports on these items:

- The required tools: `node`, `ffmpeg`, `ffprobe`, `whisper-cli` and `curl`.
- The ffmpeg subtitles (libass) filter.
- The macOS `say` command, which voice-over uses.
- The `h264_videotoolbox` and `libx264` video encoders.
- The speech model file and its size.
- Each AI tool in the `ai_cli_tools` list, with a short test.

Run it with this command.

```sh
aicw-video doctor
```

If all required tools and the subtitles filter are present, `doctor` exits with code 0. If not, it exits with code 1 and shows an install hint for each missing tool.

If `h264_videotoolbox` is missing from your ffmpeg build, AICW Video uses `libx264`. This is slower.

## Use your own tools

If a tool is not in the default place, set an environment variable. See [Configuration](/configuration).

- `FFMPEG_PATH` sets the path to `ffmpeg`.
- `FFPROBE_PATH` sets the path to `ffprobe`.
- `WHISPER_PATH` sets the path to `whisper-cli`.
- `AICW_VIDEO_WHISPER_MODEL` sets the path to a ggml speech model.
