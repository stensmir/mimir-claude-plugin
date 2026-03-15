---
name: mimir-cli
description: >-
  Use Mimir CLI for voice-to-text transcription and audio processing. Trigger this skill when the user wants to:
  transcribe audio/video files, record from microphone, convert speech to text, manage Whisper/Parakeet ASR models,
  switch transcription engines, configure Ollama or LLM post-processing for dictation, check or change Mimir settings,
  or anything involving the `mimir` command-line tool. Also trigger when the user mentions audio transcription,
  speech recognition, dictation, voice-to-text, STT, or asks about available ASR models.
---

# Mimir CLI — Voice-to-Text Transcription

Mimir is a macOS desktop app for voice dictation with a powerful CLI interface. This skill teaches you how to use the `mimir` CLI to transcribe audio, manage models, and configure the system.

## Prerequisites

The `mimir` binary must be installed. Check with:
```bash
which mimir || echo "Not installed"
```

If not installed, the user needs to either:
- Install Mimir.app and run: `sudo cp "/Applications/Mimir.app/Contents/MacOS/mimir" /usr/local/bin/mimir`
- Or build from source: `cargo build --release --bin mimir`

## Transcription

### From a file
```bash
mimir transcribe path/to/audio.wav
```

Supports common audio/video formats: wav, mp3, m4a, ogg, flac, mp4, webm, mkv.

### From microphone
```bash
mimir transcribe --duration 10    # Record 10 seconds from default mic
```

### Key options
| Flag | Description | Example |
|------|-------------|---------|
| `--engine` / `-e` | ASR engine: `whisper` (default) or `parakeet` | `-e parakeet` |
| `--model` / `-m` | Whisper model size: `tiny`, `base`, `small`, `medium` | `-m medium` |
| `--format` / `-f` | Output format: `text` (default), `json`, `srt` | `-f json` |
| `--output` / `-o` | Write to file instead of stdout | `-o result.txt` |
| `--language` / `-l` | Force language (auto-detect if omitted) | `-l ru` |
| `--quiet` / `-q` | Output only transcription text, no progress info | `-q` |
| `--no-processing` | Disable filler word removal and punctuation cleanup | |

### Engine choice guidance
- **Whisper** — general purpose, multiple model sizes, good multilingual support. Default.
- **Parakeet** — NVIDIA's model, faster on supported hardware, English-focused. Use when the user wants speed or mentions Parakeet.

### Output formats
- `text` — plain text, one paragraph. Best for pasting into documents.
- `json` — structured output with text, language, confidence, segments, timing. Best for programmatic use.
- `srt` — SubRip subtitle format with timestamps. Best for video subtitles.

## Model Management

```bash
mimir model list        # Show available models with download status
```

This shows which Whisper models (tiny/base/small/medium) are downloaded and ready to use, plus Parakeet status.

To switch the default model size:
```bash
mimir config set model_size medium
```

Model size trade-offs:
- `tiny` (75MB) — fastest, lowest quality. Good for quick drafts.
- `base` (142MB) — balanced for casual use.
- `small` (466MB) — good quality, reasonable speed.
- `medium` (1.5GB) — best quality, slowest. Use for important transcriptions.

## Switching ASR Engine

To switch between Whisper and Parakeet for a single transcription:
```bash
mimir transcribe audio.wav --engine parakeet
mimir transcribe audio.wav --engine whisper
```

The engine flag is per-command; the app defaults to Whisper.

## LLM / Ollama Configuration

Mimir can use an LLM (like Ollama) for post-processing transcriptions — improving text quality, adding terms to the dictionary automatically, and translating.

### Enable Ollama
```bash
mimir config set llm.enabled true
mimir config set llm.provider ollama
mimir config set llm.endpoint http://localhost:11434
mimir config set llm.model llama3.2:3b
```

### Switch Ollama model
```bash
mimir config set llm.model mistral:7b
```

### Switch to LM Studio
```bash
mimir config set llm.provider lmstudio
mimir config set llm.endpoint http://localhost:1234
```

### Disable LLM
```bash
mimir config set llm.enabled false
```

### Supported providers
- `ollama` — local Ollama server (default endpoint: `http://localhost:11434`)
- `lmstudio` — LM Studio (default endpoint: `http://localhost:1234`)
- `anthropic` — Anthropic API (requires `llm.api_key`)

After changing LLM settings, restart the Mimir daemon for changes to take effect.

## Configuration

### View all settings
```bash
mimir config show           # Human-readable
mimir config show --json    # Full JSON (for programmatic use)
```

### Common settings
```bash
mimir config set model_size small                    # Default Whisper model
mimir config set intelligent_processing true         # Enable text cleanup
mimir config set auto_paste true                     # Auto-paste after transcription
mimir config set sound_feedback true                 # Play sound on start/stop
```

### Show config file location
```bash
mimir config path
```

### Reset to defaults
```bash
mimir config reset
```

## Permissions (macOS)

```bash
mimir permissions --check     # Check current permission status
mimir permissions --request   # Open System Settings to grant permissions
```

Mimir needs Accessibility and Microphone permissions on macOS.

## Typical Workflows

### "Transcribe this meeting recording"
```bash
mimir transcribe meeting.m4a --model medium --quiet
```

### "Switch to Parakeet and transcribe"
```bash
mimir transcribe audio.wav --engine parakeet --quiet
```

### "Set up Ollama for dictionary training"
```bash
mimir config set llm.enabled true
mimir config set llm.provider ollama
mimir config set llm.endpoint http://localhost:11434
mimir config set llm.model llama3.2:3b
```

### "What models do I have?"
```bash
mimir model list
```

### "Get subtitles for this video"
```bash
mimir transcribe video.mp4 --format srt --output video.srt
```
