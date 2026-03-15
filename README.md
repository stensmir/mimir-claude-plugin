# Mimir Claude Code Plugin

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin for voice-to-text transcription using [Mimir](https://github.com/stensmir/Mimir).

## Installation

```bash
claude plugin add https://github.com/stensmir/mimir-claude-plugin
```

## Prerequisites

Install the Mimir CLI:

```bash
# If Mimir.app is installed:
sudo cp "/Applications/Mimir.app/Contents/MacOS/mimir" /usr/local/bin/mimir
sudo chmod +x /usr/local/bin/mimir
```

## What it does

This plugin teaches Claude how to use the `mimir` CLI for:

- **Transcribe audio/video files** — `mimir transcribe recording.m4a`
- **Record from microphone** — `mimir transcribe --duration 10`
- **Switch ASR engines** — Whisper (multilingual) or Parakeet (fast, English)
- **Manage models** — list, download, switch Whisper model sizes
- **Configure Ollama/LLM** — enable post-processing, switch models
- **View/change settings** — `mimir config show/set`

## Example prompts

- "Transcribe this audio file"
- "Switch to Whisper medium model"
- "Set up Ollama with mistral:7b for Mimir"
- "What ASR models do I have?"
- "Generate subtitles for this video"

## Links

- [Mimir App](https://github.com/stensmir/Mimir) — macOS voice dictation app
- [Claude Code Plugins](https://docs.anthropic.com/en/docs/claude-code/plugins)
