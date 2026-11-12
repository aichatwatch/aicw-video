---
title: Configuration
description: Settings in config.json and the environment variables that AICW Video reads.
---

## config.json

AICW Video reads settings from the file `config.json` in the root folder of the installed package. A source checkout has the same file in the repository root. If a key is missing, AICW Video uses the default. If the file is missing or not valid JSON, AICW Video uses all defaults.

Number values outside the allowed range are moved to the nearest limit.

### Common settings

| Key | Default | Meaning |
| --- | --- | --- |
| `caption_language` | `English` | The language of captions that AI proofreads or translates. AICW Video keeps the raw whisper text as `original_text`. |
| `ai_cli_tools` | See below | The ordered list of AI tools for standalone use. |
| `renderDefaults` | See below | The settings that a new clip starts with. |

### renderDefaults

| Key | Default | Meaning |
| --- | --- | --- |
| `caption_style` | `bold-white-bottom` | The caption look and place. |
| `caption_animation` | `word-highlight` | The caption motion: `static`, `word-pop` or `word-highlight`. |
| `reframe` | `letterbox-blur` | How the clip fits the target shape: `crop` or `letterbox-blur`. |

The caption styles are `plain`, `tiktok-yellow`, `bold-white`, `neon`, `pastel`, `beast-impact`, `creator-thin` and `karaoke`. Add `-bottom` to the name, for example `neon-bottom`. The plan UI shows each style as a top version and a bottom version.

### ai_cli_tools

This list sets which AI tools the standalone commands try, and in which order. When a tool is not available or it fails, AICW Video tries the next one. When an MCP host is connected, the host model does the AI work instead.

The default list is:

```json
{
  "ai_cli_tools": [
    { "name": "claude-code", "command": "claude", "supports_images": true },
    { "name": "codex", "command": "codex", "supports_images": true },
    { "name": "ollama", "command": "ollama", "model": "gemma4", "supports_images": false },
    { "name": "ollama", "command": "ollama", "model": "qwen3.6", "supports_images": false }
  ]
}
```

Each entry can have these keys:

- `name`: the tool id, for example `claude-code`, `codex` or `ollama`.
- `command`: the program name or the full path.
- `args`: fixed arguments to add.
- `model`: the model, for tools that need one.
- `enabled`: set to `false` to keep an entry in the file but skip it.
- `supports_images`: set to `true` if the tool can receive images.

For a local-only setup, see [Privacy and AI use](/privacy-and-ai-use).

### Advanced settings

| Key | Default | Allowed range |
| --- | --- | --- |
| `debugMode` | `false` | `true` or `false` |
| `ffmpegDebug` | `false` | `true` or `false` |
| `aiCliTimeoutMs` | `120000` | 10000 to 1800000 |
| `faceEmojiScale` | `1.05` | 0.5 to 3 |
| `visualSampleWithAudioIntervalMs` | `2000` | 500 to 60000 |
| `visualSampleWithoutAudioIntervalMs` | `4000` | 500 to 60000 |
| `visualSampleImageMaxLongEdge` | `512` | 256 to 1600 |
| `visualAiMaxFramesPerCall` | `12` | 1 to 40 |
| `illustrationPromptTemplatePath` | `config/illustration-prompt-template.md` | A file path |

- `debugMode` keeps verbose output from subprocesses.
- `ffmpegDebug` keeps the detailed ffmpeg messages.
- `aiCliTimeoutMs` is the longest wait for one AI tool request, in milliseconds. After that time, AICW Video tries the next tool.
- `faceEmojiScale` multiplies the size of the emoji that covers a face.
- `visualSampleWithAudioIntervalMs` is the time between sampled frames for a video that has speech.
- `visualSampleWithoutAudioIntervalMs` is the same for a video without usable audio.
- `visualSampleImageMaxLongEdge` is the longest edge, in pixels, of a sampled frame.
- `visualAiMaxFramesPerCall` is the most frames that AICW Video sends to an AI tool in one request.
- `illustrationPromptTemplatePath` is the prompt template for generated side illustrations.

## Environment variables

### Tools

- `FFMPEG_PATH`: the path to `ffmpeg`.
- `FFPROBE_PATH`: the path to `ffprobe`.
- `WHISPER_PATH`: the path to `whisper-cli`. The default is `whisper-cli`.
- `AICW_VIDEO_WHISPER_MODEL`: the path to a ggml speech model. If you set it and the file does not exist, transcription fails. The default is `~/.cache/aicw-video/ggml-base.en.bin`.
- `AICW_VIDEO_CLAUDE_PATH`: the path to the `claude` program. The default is `claude`.
- `AICW_VIDEO_HYPERFRAMES_BIN`: the path to the program that makes side illustrations.
- `EDITOR`: the editor that `plan` opens when you answer `e`. The default is `vi`.

### Project

- `AICW_VIDEO_PROJECT`: the default project folder for commands.

### Render

- `AICW_VIDEO_ENCODER`: the video encoder. On macOS, the default is `h264_videotoolbox`, which uses the hardware encoder. On other systems, the default is `libx264`. Example values are `libx264` and `h264_videotoolbox`.
- `AICW_VIDEO_RENDER_CONCURRENCY`: the number of clips that render at the same time. The default is 4. It is never more than the number of clips. The `--concurrency` option takes priority.

### Behavior

- `AICW_VIDEO_NO_HOME`: set to `1` to run the MCP server when you start `aicw-video` with no arguments in a terminal.
- `AICW_VIDEO_VERBOSE`: set to `1` to show the full `doctor` report when you start the interactive mode.
- `AICW_VIDEO_PROOFREAD_CAPTIONS`: set to `0` to turn off AI proofreading of captions.
- `AICW_VIDEO_DEBUG`, `AICW_VIDEO_FFMPEG_DEBUG` and `AICW_VIDEO_AI_TIMEOUT_MS` override `debugMode`, `ffmpegDebug` and `aiCliTimeoutMs`.

AICW Video also reads a `.env` file in the package root folder. A variable that is already set in your shell wins over the file.

## Where files go

| Item | Location |
| --- | --- |
| Projects | `~/aicw-video/projects/` |
| Speech model | `~/.cache/aicw-video/` |
