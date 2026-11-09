# Meeting Recorder

Last Updated: November 9, 2026

Single-file bash script that records a meeting's microphone and system audio as separate FLAC tracks, then transcribes both and merges them into one timestamped, speaker-labeled transcript.

## Behavior

### Startup

1. `umask 077` — all output is owner-only.
2. Creates the output directory `~/Documents/Transcripts`.
3. Checks dependencies in order: `ffmpeg`, `python3`, Python module `faster_whisper`. A missing dependency prints an install hint and exits 1.
4. Resolves the session name: first CLI argument, else an interactive prompt, else `meeting`.
5. Sanitizes the name: spaces to underscores, keep only `[[:alnum:]_-]`.

### Recording

- Two temp FLAC files in `/tmp/meeting_rec.*` — one per source, removed on exit.
- ffmpeg reads the mic from the `default` PulseAudio/PipeWire source and callers from the `@DEFAULT_MONITOR@` monitor.
- `-nostdin` keeps ffmpeg off stdin so `Ctrl+C` reaches the script, not ffmpeg.
- Runs until `SIGINT`.

### Stop and transcribe

`SIGINT` (`Ctrl+C`) triggers `cleanup_and_transcribe`:

- If both temp files exist, moves them to `~/Documents/Transcripts/<timestamp>-<name>.{mic,speaker}.flac`.
- Runs an inline Python faster-whisper transcription:
  - model `base`, `device="cpu"`, `compute_type="float32"`, `cpu_threads=16`.
  - `beam_size=5`, `word_timestamps=True`, `condition_on_previous_text=False`.
  - Words from both channels are merged by speaker label into lines `[HH:MM:SS] <Label>: <text>`.
- Writes the merged transcript to `<timestamp>-<name>.txt`.
- Opens `~/Documents/Transcripts` in the default file manager (`gio open`, best-effort).
- Exits 0.

### Fallback

If ffmpeg exits without `SIGINT` (e.g. an audio-source error), `cleanup_and_transcribe` runs once; with no captured audio it prints `No recording was captured.`

## Output files

Per session, in `~/Documents/Transcripts/`:

- `<timestamp>-<name>.mic.flac` — the user's microphone.
- `<timestamp>-<name>.speaker.flac` — system output (callers).
- `<timestamp>-<name>.txt` — merged, timestamped, speaker-labeled transcript.

## Configuration

Hardcoded near the top of the script: `SAVE_DIR`, `WHISPER_MODEL`, `MIC_LABEL`, `SPEAKER_LABEL`.
