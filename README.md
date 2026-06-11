# artts — Article-to-Speech pipeline

Reads articles through a local LLM (Ollama) + TTS (edge-tts) → MP3.  
Built for Android/Termux.

## Quick start

```bash
# On your phone in Termux:
gh repo clone JP-matstat/artts ~/.local/share/artts
mkdir -p ~/.local/bin
ln -s ~/.local/share/artts/artts ~/.local/bin/artts

# First run — installs Ollama, pulls models, installs deps:
artts setup

# Use it:
artts read article.txt
artts read https://example.com/article
cat notes.txt | artts read
artts read -m llama3.2:1b -o podcast.mp3 https://example.com/post
```

> **Important:** Go to **Settings → Developer Options** and enable  
> **"Disable child process restrictions"** — required for Ollama in Termux.

## Requirements

- Samsung S25 (or any Android 12+ with 6GB+ RAM)
- [Termux](https://f-droid.org/packages/com.termux/) (from F-Droid, **not** Play Store)
- 2–3 GB free storage for models

## Commands

| Command | What it does |
|---------|-------------|
| `artts setup` | Install Ollama, pull LLM model, install edge-tts |
| `artts read [src]` | Article → LLM rewrite → TTS → MP3 |
| `artts voices` | List available edge-tts voices |
| `artts models` | List installed Ollama models |

### `artts read` options

| Flag | Default | Description |
|------|---------|-------------|
| `-m` | `qwen2.5:1.5b` | Ollama LLM model |
| `-v` | `en-US-JennyNeural` | TTS voice |
| `-o` | `{input}.mp3` | Output MP3 file |
| `-p` | (default prompt) | Custom LLM instruction |
| `-d` | off | Skip LLM, TTS-only mode |

## How it works

1. Reads article from file, URL, or stdin
2. LLM rewrites it as a natural audio script
3. edge-tts converts to speech → saves as MP3

## Full-offline alternative

If you want no internet dependency for TTS, `artts` supports the [Orpheus TTS](https://ollama.com/legraphista/Orpheus) model via Ollama. After pulling it (`ollama pull legraphista/Orpheus`), use `-m legraphista/Orpheus` to route through it instead of edge-tts.
