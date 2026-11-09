# Meeting Recorder

Records a meeting's microphone and system audio as two separate FLAC tracks, then transcribes both with faster-whisper into one timestamped, speaker-labeled transcript.

## What it does

- Records your mic from the `default` PulseAudio/PipeWire source.
- Records the callers from the `@DEFAULT_MONITOR@` monitor (system output).
- On `Ctrl+C`, saves both tracks, transcribes them, merges them into one labeled file, and opens the transcript folder.

## Requirements

- `ffmpeg` — `sudo dnf install ffmpeg-free`
- `python3`
- `faster-whisper` — `python3 -m pip install --user faster-whisper`

## Install

```sh
ln -s ~/workspace/meeting-recorder/meeting-recorder ~/.local/bin/meeting-recorder
```

## Usage

```sh
meeting-recorder "standup"   # optional session name
```

Press `Ctrl+C` when the meeting ends. Audio and transcripts are written to `~/Documents/Transcripts/`.

## Desktop integration

- Dash icon: `meeting-recorder.desktop` in `~/.local/share/applications/`.
- Keyboard shortcut: `Super+r` opens it in a terminal.
