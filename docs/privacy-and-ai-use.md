---
title: Privacy and AI use
description: What AICW Video does on your computer, what an AI tool can see, and how to use a local model.
---

AICW Video has no telemetry. It makes no network calls except to the tools that it starts. The one download is the speech model. AICW Video gets it with `curl` the first time that you transcribe. Its local web servers listen on `127.0.0.1` only.

## What runs on your computer

AICW Video does these jobs locally:

- It extracts audio and video.
- It transcribes speech with whisper.
- It detects faces with TensorFlow, for the privacy features. Face detection never needs a cloud AI.

If AI scene analysis is off, AICW Video skips visual descriptions. It uses local whisper and local face detection only.

## What AI is used for

AI scene analysis can do these jobs:

- suggest clip ranges;
- label key frames;
- write visual captions for silent video;
- proofread captions, if you do not turn this off.

To turn off caption proofreading, set `AICW_VIDEO_PROOFREAD_CAPTIONS` to `0`.

## What an AI tool can see

When you turn on AI scene analysis, AICW Video uses the AI tools that you already have:

- **Claude Code or Claude CLI.** AICW Video starts it as a command.
- **Codex CLI.** AICW Video can start it as a command.
- **Ollama.** You can set it as a local fallback in `config.json`.
- **MCP host sampling.** Claude Code, Claude Desktop and Codex call the local MCP server, and AICW Video asks the host model for text. ChatGPT needs a remote MCP URL.

<Warning>
If you use Claude Code, Codex or another cloud AI host, the host tool can send sampled frames, transcript snippets and caption text to its provider. Check the privacy terms of your provider.
</Warning>

If you want no cloud AI, turn off AI scene analysis, or use only Ollama as shown below.

## Use a local Ollama model

Ollama runs a model on your computer. The Ollama adapter of AICW Video handles text only. It cannot read image frames yet. So visual frame descriptions still need Claude Code, Codex or an MCP host that supports sampling. Keep `supports_images` set to `false` for Ollama.

Install Ollama.

```sh
brew install ollama
```

Start the Ollama server.

```sh
ollama serve
```

Download the first default model.

```sh
ollama pull gemma4
```

Download the second default model.

```sh
ollama pull qwen3.6
```

By default, AICW Video tries `gemma4` and then `qwen3.6` after Claude Code and Codex are not available or fail. To make Ollama the only fallback, set `ai_cli_tools` in `config.json` to this list.

```json
{
  "ai_cli_tools": [
    { "name": "ollama", "command": "ollama", "model": "gemma4", "supports_images": false },
    { "name": "ollama", "command": "ollama", "model": "qwen3.6", "supports_images": false }
  ]
}
```

See [Configuration](/configuration) for the other settings.

## Privacy features for the clips

You can protect people in the finished clips:

- **Face blur or emoji.** Blur detected faces with a soft or strong blur, or cover them with an emoji. The emoji feature is alpha.
- **Voice-over.** Replace the original voice of the clip with a computer voice. This feature is alpha. It works on macOS only, and it uses the system text-to-speech engine.

Check the final render before you publish a clip. The caption preview in the plan UI is approximate.
