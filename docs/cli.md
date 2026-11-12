---
title: CLI reference
description: Every aicw-video command, its options and the environment variables that it reads.
---

Run `aicw-video help` to see the built-in help.

```sh
aicw-video help
```

## How a command finds the project

Most commands work on one project folder. A command finds the folder in this order:

1. The `--project` option.
2. The `AICW_VIDEO_PROJECT` environment variable.
3. The current folder.

The `import` command is different. If you set no project, `import` makes a folder in `~/aicw-video/projects/` from the name of the video.

In the lists below, `--project <path>` means this option. The option is not required.

## Start the app

### aicw-video

Run the command with no arguments. In a terminal, it starts the hub and opens your browser. If the input and output are piped, as when Claude Code starts it, it runs the MCP server. If you set `AICW_VIDEO_NO_HOME=1`, it runs the MCP server also in a terminal.

```sh
aicw-video
```

### home

Start the hub. The hub shows the MCP setup and lists your projects. It tries port 8764 first. Press Ctrl-C to stop it.

```sh
aicw-video home
```

### menu

Open the interactive menu.

```sh
aicw-video menu
```

### interactive

Start a guided prompt. It asks for a video path and then offers each step. The short name is `i`.

```sh
aicw-video interactive
```

### doctor

Check your tools, encoders, speech model and AI tools. It exits with code 0 if all required tools are present, and with code 1 if not. See [Requirements](/requirements).

```sh
aicw-video doctor
```

## Run the pipeline

### init

Create an empty project folder at the path that you give.

```sh
aicw-video init ~/aicw-video/projects/my-video
```

### import

Copy a local video file into a project.

```sh
aicw-video import /path/to/video.mov --project ~/aicw-video/projects/my-video
```

### transcribe

Extract the audio and transcribe it with whisper. The command writes sentence timing and word timing. Options: `--project <path>`.

```sh
aicw-video transcribe --project ~/aicw-video/projects/my-video
```

### analyze

Find scenes and key frames, and pair them with the transcript. The command writes `analysis/moments.json`. If the analysis exists, the command reuses it. Options: `--project <path>` and `--force` to run it again.

```sh
aicw-video analyze --force
```

### go

Run all steps in one command: audio, transcription, analysis, AI description, AI clip plan, clip suggestions, and the plan UI. The command skips steps that are already done. It skips the AI steps if no AI tool is available. Options: `--project <path>`.

```sh
aicw-video go
```

### status

Show which steps are done and what comes next.

```sh
aicw-video status
```

### where

Print the absolute path of the project folder. Use it in shell scripts.

```sh
aicw-video where
```

### reveal

Open the project folder in Finder.

```sh
aicw-video reveal
```

## Plan and render

### suggest

Score clip candidates with a heuristic and write `shorts/suggestions.json`. Options: `--count <n>` and `--duration <seconds>`.

```sh
aicw-video suggest --count 5 --duration 25
```

### plan

Show `shorts/plan.json`. Then the command asks if you want to render all clips. Answer `y` to render, `e` to edit the plan in your `EDITOR` (or `vi`), or anything else to stop. If there is no plan, the command stops with exit code 2. Add `--no-render` to show the plan and stop.

```sh
aicw-video plan --no-render
```

### plan-ui

Build the plan UI, start a local server for it, and open it in your browser. Press Ctrl-C to stop the server.

```sh
aicw-video plan-ui
```

### render

Render clips from `shorts/plan.json`. By default, it renders all clips. Options: `--clip <id>` for one clip, and `--concurrency <n>` for the number of clips that it renders at the same time.

```sh
aicw-video render --clip 01
```

### thumbnail

Render a JPG cover for each clip, or for one clip with `--clip <id>`.

```sh
aicw-video thumbnail --clip 01
```

### tutorial

Build a step-by-step tutorial from the analysis. Options: `--title <text>`, `--steps <n>`, and `--format <html|md|both>`. The default format is `both`.

```sh
aicw-video tutorial --title "My tutorial" --format md
```

## AI steps

These commands use the AI tools in the `ai_cli_tools` list, in order. See [Configuration](/configuration) and [Privacy and AI use](/privacy-and-ai-use).

### describe

Make a summary and a caption for each key frame, and write `description.json`. Option: `--frame-count <n>`. The default is 6.

```sh
aicw-video describe --frame-count 6
```

### plan-clips

Ask the AI to propose a clip plan, and write `shorts/plan.json`. Options: `--count <n>` (default 3), `--duration <seconds>` (default 25) and `--hint <text>`.

```sh
aicw-video plan-clips --count 3 --duration 25
```

## Audio tools

### match-audio

Scan a folder and pair each video with the audio file that matches its speech. The command prints the pairs and any audio files without a pair.

```sh
aicw-video match-audio --folder /path/to/folder
```

### replace-audio

Align an external audio file to the project video by matching the transcript. The command writes a new file and prints the time offset.

```sh
aicw-video replace-audio --audio /path/to/audio.wav
```

### swap-source

Make the replaced audio version the source of the project. The only value that `--to` accepts is `replaced`.

```sh
aicw-video swap-source --to replaced
```

## Connect an AI agent

These commands only print text. They do not change your files. See [Use with AI agents](/use-with-ai-agents).

- `setup-mcp` prints the setup for Claude Code, Claude Desktop, Codex and ChatGPT.
- `setup-claude-code` prints the `claude mcp add` line for Claude Code.
- `setup-claude-desktop` prints the JSON for `claude_desktop_config.json`.
- `setup-codex` prints the TOML for `~/.codex/config.toml`.
- `setup-chatgpt` prints notes on ChatGPT and remote MCP.

To print all of them, run this command.

```sh
aicw-video setup-mcp
```

### mcp

Run the MCP server on standard input and output. An AI host starts this command. See [MCP tools](/mcp-tools).

```sh
aicw-video mcp
```

## Environment variables

See [Configuration](/configuration) for the full list.
