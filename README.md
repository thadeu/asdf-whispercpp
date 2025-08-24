# asdf-whispercpp

Plugin for [ASDF](https://asdf-vm.com/) and [MISE](https://mise.jdx.dev/) that installs [whisper.cpp](https://github.com/ggml-org/whisper.cpp) - an optimized implementation of OpenAI's Whisper for audio transcription.

## 📋 Prerequisites

### System Dependencies

#### macOS
```bash
# Install via Homebrew
brew install cmake git curl

# Or via MacPorts
sudo port install cmake git curl
```

#### Ubuntu/Debian
```bash
# Update repositories
sudo apt update

# Install dependencies
sudo apt install -y cmake git curl build-essential
```

#### Other Linux distributions
```bash
# Fedora/RHEL/CentOS
sudo dnf install cmake git curl gcc-c++

# Arch Linux
sudo pacman -S cmake git curl base-devel
```

## 🚀 Installation

### Install the plugin

#### For ASDF:
```bash
asdf plugin add whispercpp https://github.com/seu-usuario/asdf-whispercpp.git
```

#### For MISE:
```bash
mise plugin add whispercpp https://github.com/seu-usuario/asdf-whispercpp.git
```

### Install whisper.cpp

```bash
# Install latest version
asdf install whispercpp latest
# or
mise install whispercpp@latest

# Install specific version
asdf install whispercpp v1.5.4
# or
mise install whispercpp@v1.5.4

# Set as global version
asdf global whispercpp latest
# or
mise use -g whispercpp@latest
```

## 📦 What gets installed

The plugin installs:

- **`whisper`** - Main wrapper that automatically manages models
- **`whisper-cli`** - Main whisper.cpp binary
- **`whisper-server`** - HTTP server for transcription

## 🎯 How it works

### Automatic model management

The `whisper` wrapper automatically downloads models from Hugging Face when needed:

- Models are saved in `~/.local/share/mise/whisper-models/`
- Supported formats: `ggml-{model}.bin`
- Source: [ggerganov/whisper.cpp](https://huggingface.co/ggerganov/whisper.cpp)

### Available models

- `tiny` - 39 MB, fastest, lower accuracy
- `base` - 74 MB, speed/accuracy balance
- `small` - 244 MB, good accuracy
- `medium` - 769 MB, high accuracy
- `large` - 1550 MB, maximum accuracy
- `large-v2` - 1550 MB, improved large version
- `large-v3` - 1550 MB, latest version

## 💻 Usage examples

### Basic transcription
```bash
# Transcription with default model (base)
whisper audio.mp3

# Specify model
whisper -m tiny audio.mp3
whisper -m large audio.wav
```

### Output options
```bash
# Save to text file
whisper audio.mp3 -o transcript.txt

# JSON format with timestamps
whisper audio.mp3 -f json -o output.json

# SRT format (subtitles)
whisper audio.mp3 -f srt -o subtitles.srt

# VTT format (WebVTT)
whisper audio.mp3 -f vtt -o subtitles.vtt
```

### Language settings
```bash
# Force specific language
whisper audio.mp3 -l pt
whisper audio.mp3 -l en
whisper audio.mp3 -l es

# Automatic language detection
whisper audio.mp3 -l auto
```

### Batch processing
```bash
# Process multiple files
whisper *.mp3

# With specific model
whisper -m large *.wav *.mp3 *.m4a
```

### Advanced options
```bash
# Adjust threads (default: auto)
whisper audio.mp3 -t 4

# Adjust context (default: 30)
whisper audio.mp3 -c 60

# Adjust temperature (default: 0.0)
whisper audio.mp3 --temperature 0.2

# Verbose output
whisper audio.mp3 -v

# Translate to English only
whisper audio.mp3 --translate
```

### Using the HTTP server
```bash
# Start server
whisper-server --host 0.0.0.0 --port 8080

or

whisper-server --model ~/.local/share/mise/whisper-models/ggml-small-q5_1.bin --port 8080 -l pt -nt -ng

# Use via curl
curl 127.0.0.1:8001/inference \
    -H "Content-Type: multipart/form-data" \
    -F file="@audio.wav" \
    -F temperature="0.0" \
    -F temperature_inc="0.2" \
    -F response_format="json"
```

### Using whisper-cli directly
```bash
# If you want to use the original binary
whisper-cli -m ~/.local/share/mise/whisper-models/ggml-base.bin audio.mp3
```

## 📁 File structure

```
~/.local/share/mise/whisper-models/
├── ggml-tiny.bin
├── ggml-base.bin
├── ggml-small.bin
├── ggml-medium.bin
├── ggml-large.bin
├── ggml-large-v2.bin
└── ggml-large-v3.bin
```

## 🐛 Troubleshooting

### Model download error
```bash
# Check connectivity
curl -I https://huggingface.co/ggerganov/whisper.cpp

# Manual download
curl -L -o ~/.local/share/mise/whisper-models/ggml-base.bin \
  https://huggingface.co/ggerganov/whisper.cpp/resolve/main/ggml-base.bin
```

### Permission issues
```bash
# Check permissions
ls -la ~/.local/share/mise/whisper-models/

# Fix permissions
chmod 755 ~/.local/share/mise/whisper-models/
```

## 📚 Additional resources

- [Official whisper.cpp documentation](https://github.com/ggml-org/whisper.cpp)
- [Available models on Hugging Face](https://huggingface.co/ggerganov/whisper.cpp/tree/main)
- [Performance tuning guide](https://github.com/ggml-org/whisper.cpp#performance-tuning)

## 🤝 Contributing

Contributions are welcome! Please open an issue or pull request.

## 📄 License

This project is licensed under the same license as whisper.cpp.