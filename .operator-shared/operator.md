# Shared Operator Instructions

## Overview

Single-file bash helper that records a meeting's microphone and system audio into separate FLAC tracks and transcribes them with faster-whisper into one speaker-labeled transcript.

## Project Rules

- Keep `meeting-recorder` a single portable bash script; its only runtime dependencies are the three checked at startup.
- Run `bash -n meeting-recorder` after edits to syntax-check.
- Match the existing shell style: `set -e`, `umask 077`, heredoc for the inline Python, `command -v` for dependency checks.
- Keep the transcription logic inline so the script stays one file.
- Commit messages: `<type>: <summary>` (e.g. `fix:`, `feat:`, `docs:`).

## Core Philosophy

- Audio stays separable (mic vs system output) so transcription can label speakers.
- Stopping is user-driven via `SIGINT` (`Ctrl+C`); the trap is the single cleanup and transcription path.
- Output is owner-only (`umask 077`) because recordings are sensitive.
