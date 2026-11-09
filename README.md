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

Optional. Two ways to start the recorder from the desktop; both use the installed symlink at `~/.local/bin/meeting-recorder`.

### Dash icon

Symlink the bundled `meeting-recorder.desktop` into the applications directory, then pin it to the dash:

```sh
ln -s ~/workspace/meeting-recorder/meeting-recorder.desktop ~/.local/share/applications/meeting-recorder.desktop
```

Then press `Super`, search "Meeting Recorder", right-click, and choose "Pin to Dash".

### Keyboard shortcut (`Super+r`)

Bind `Super+r` to open the recorder in a terminal:

```sh
gsettings set org.gnome.settings-daemon.plugins.media-keys custom-keybindings \
  "['/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/']"
gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ name 'Meeting Recorder'
gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ command 'gnome-terminal --title "Meeting Recorder" -- /home/matthias-conteo/.local/bin/meeting-recorder'
gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ binding '<Super>r'
```

If you already have custom keybindings, use a new slot (`custom1`, `custom2`, ...) and append its path to the `custom-keybindings` list instead of overwriting.
