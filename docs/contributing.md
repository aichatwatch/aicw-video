---
title: Contributing
description: How to report bugs, report security problems and send changes to AICW Video.
---

AICW Video is a small, focused project. A change that keeps it small is welcome. The project uses the MIT license. The full rules are in the files [CONTRIBUTING.md](https://github.com/aichatwatch/aicw-video/blob/main/CONTRIBUTING.md) and [SECURITY.md](https://github.com/aichatwatch/aicw-video/blob/main/SECURITY.md) in the repository.

## Report a bug

Open an issue on [GitHub](https://github.com/aichatwatch/aicw-video/issues/new). Include this information:

- your macOS version and chip (Intel or Apple Silicon);
- the output of `aicw-video doctor`;
- the exact prompt or command that caused the problem;
- for a render problem, the file `shorts/plan.json` of the project.

## Report a security problem

Do not open a public issue. Use the **Report a vulnerability** link on the **Security** tab of the GitHub repository. The maintainers aim to reply within 5 working days.

These items are in scope:

- the code in the repository;
- the local web servers `home-server` and `plan-server`, which listen on `127.0.0.1`: cross-origin attacks, path traversal and command injection;
- the way AICW Video starts other programs: argument injection and path traversal.

These items are out of scope:

- bugs in other software that AICW Video uses, such as ffmpeg, whisper.cpp and the MCP SDK: report them to those projects;
- misuse of the tool on content that the user does not own;
- problems that come from untrusted videos with a changed setup, for example a `plan.json` that someone edited to hold bad ffmpeg filter expressions.

AICW Video is a tool for one user on one computer. It does not host content. It accepts connections from `127.0.0.1` only, and it sends no telemetry. The security policy assumes three things:

1. You trust the videos that you import.
2. You trust the AI host that you connect.
3. You are the only person with shell access to your computer.

If your setup breaks one of these, AICW Video may not be safe for you.

## Send a change

Clone the repository and build it.

```sh
git clone https://github.com/aichatwatch/aicw-video
```

```sh
cd aicw-video
```

```sh
npm install
```

```sh
npm run build
```

Run it from your clone.

```sh
node dist/cli.js doctor
```

Before you send a pull request, check this list:

- `npm run build` ends without errors. The code uses TypeScript strict mode.
- The `doctor` command still runs cleanly on macOS.
- If you changed the plan UI, open it with a real project and use what you changed.
- If you changed the renderer, render one clip from start to end. Check that the captions still line up.
- If you add an environment variable, add it to the README.
- If a change affects privacy or rights, for example it uploads data or calls a new third-party service, describe it in `NOTICE.md`.

## Project rules

- The project is macOS first. Do not add cross-platform code unless a feature needs it.
- Keep the project focused. Ship one working path, and add others only when a second real use appears.
- Add no telemetry. Make no network calls except to the third-party tools that the project already starts. Bind every HTTP server to `127.0.0.1`.
- Write plain code. Each pipeline stage writes structured JSON, and later stages read it. Keep that simple.
- Add no new top-level abstraction without two real uses.

## License

AICW Video uses the [MIT license](https://github.com/aichatwatch/aicw-video/blob/main/LICENSE).
