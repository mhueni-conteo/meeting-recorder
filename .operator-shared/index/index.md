---
description: Meeting Recorder repository and codebase navigation
read_if: Working anywhere in this repository
---

# Shared Project Index

## Guidelines

- Index all files and directories covered by this index.
- Maintain when project files or layout change.

## Architecture

- Single bash script, `meeting-recorder`, at the repository root. No build system or third-party modules.
- Records mic (`default`) and system output (`@DEFAULT_MONITOR@`) as separate FLAC files, then an inline faster-whisper transcription merges them into one labeled transcript.

## Project Index

### Repository root

- `meeting-recorder` - The recorder/transcriber script; the only source file and entry point.
- `meeting-recorder.desktop` - Optional GNOME dash launcher; install by symlinking into `~/.local/share/applications/`.
- `README.md` - User-facing overview, requirements, install, usage, and desktop integration.
- `.gitignore` - Ignores the private Operator partition and Python/editor artifacts.
