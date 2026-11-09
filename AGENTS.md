# Agent Instructions

## Project Goal

A single-file bash helper that records a meeting's microphone and system audio into two separate FLAC tracks, then transcribes both with faster-whisper into one timestamped, speaker-labeled transcript.

## Architecture

- One portable bash script, `meeting-recorder`, at the repository root. No build system or third-party modules.
- Records mic (`default`) and system output (`@DEFAULT_MONITOR@`) as separate FLAC files so transcription can label speakers.
- Inline faster-whisper transcription merges both channels into one labeled transcript.
- Stopping is user-driven via `SIGINT` (`Ctrl+C`); the trap is the single cleanup-and-transcribe path.

## Project Rules

- Keep `meeting-recorder` a single portable bash script; its only runtime dependencies are the three checked at startup (`ffmpeg`, `python3`, `faster-whisper`).
- Run `bash -n meeting-recorder` after edits to syntax-check.
- Match the existing shell style: `set -e`, `umask 077`, heredoc for the inline Python, `command -v` for dependency checks.
- Keep the transcription logic inline so the script stays one file.
- Output stays owner-only (`umask 077`) because recordings are sensitive.

## Commit Messages

`<type>: <summary>` (e.g. `fix:`, `feat:`, `docs:`).
