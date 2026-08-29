# ASTER AI — Local-First Desktop AI Application

A cross-platform desktop AI application built with Tauri (Rust frontend) and Python backend, featuring NVIDIA NIM integration for cloud models and local Ollama support. Designed for privacy: models, conversations, and data stay on your machine.

## What is it?

A native desktop app for chatting with AI models — both cloud (via NVIDIA NIM) and local (via Ollama). The Tauri/Rust frontend provides native performance and system integration, while the Python/FastAPI backend handles model orchestration, authentication, and data persistence. Everything runs locally; no conversation data leaves your machine unless you explicitly use cloud models.

## What does it do?

- **Multi-provider model support**: NVIDIA NIM (cloud, OAuth authentication) and Ollama (fully local)
- **Model selection UI**: Browse, select, and switch between available models
- **Persistent conversations**: SQLite-backed chat history with full-text search
- **Cross-platform**: Windows (installer + portable), Linux (AppImage, binary, tar.gz), macOS (planned)
- **Local-first architecture**: Python backend + Tauri frontend; all data in `~/.config/aster/` and `~/.local/share/aster/`
- **Desktop integration**: System tray, notifications, auto-start, global hotkey (planned)
- **Build pipeline**: Automated scripts for Windows (PyInstaller + Inno Setup) and Linux (Tauri CLI)

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Tauri App (Rust)                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │   Frontend  │  │  Backend    │  │   System Integration│  │
│  │  (React/TS) │◀─│  (Python)   │─▶│  Tray, Hotkeys, FS  │  │
│  └─────────────┘  └──────┬──────┘  └─────────────────────┘  │
└─────────────────────────┼───────────────────────────────────┘
                          │ IPC (Tauri)
                          ▼
                 ┌─────────────────┐
                 │  Python Backend │
                 │   (FastAPI)     │
                 │  ┌───────────┐  │
                 │  │ Providers │  │
                 │  │ NIM/Ollama│  │
                 │  └───────────┘  │
                 │  ┌───────────┐  │
                 │  │  SQLite   │  │
                 │  │  (chats,  │  │
                 │  │  settings)│  │
                 │  └───────────┘  │
                 └─────────────────┘
```

| Layer | Technology |
|-------|------------|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Desktop Shell | Tauri 2 (Rust) |
| Backend | Python 3.11+, FastAPI, uvicorn, aiosqlite |
| Local LLM | Ollama (any local model) |
| Cloud LLM | NVIDIA NIM (OAuth 2.0, OpenAI-compatible API) |
| Storage | SQLite (WAL mode), local config directories |
| Build | Windows: PyInstaller + Inno Setup / Linux: Tauri CLI (AppImage, binary) |

## Quick Start

### Local Development

```bash
# Clone and run launcher (sets up venvs, installs deps, starts backend)
python aster_desktop_launcher.py
```

### Building for Distribution

#### Windows
```bash
scripts/build_windows.sh
# Outputs: release/windows/installer/ASTER_Windows_Installer.exe
#          release/windows/portable/ASTER_Windows_Portable.exe
```

#### Linux
```bash
scripts/build_linux.sh
# Outputs: release/linux/binary/ASTER_Linux
#          release/linux/appimage/ASTER_Linux.AppImage
#          release/linux/tar.gz/ASTER_Linux.tar.gz
```

## NVIDIA Authentication Flow

1. **First run**: Welcome screen with "Connect NVIDIA" button
2. **OAuth**: Click to authorize NVIDIA access in browser
3. **Token exchange**: OAuth tokens exchanged for NVIDIA API access
4. **Model selection**: Browse available NVIDIA models
5. **Ready**: Start chatting with selected models

Tokens stored encrypted in local config; no credentials in code.

## Technologies

- **Frontend**: React, TypeScript, Vite, Tailwind CSS
- **Desktop**: Tauri 2 (Rust)
- **Backend**: Python 3.11+, FastAPI, uvicorn, aiosqlite
- **Local LLM**: Ollama
- **Cloud LLM**: NVIDIA NIM (OAuth 2.0)
- **Storage**: SQLite (local-first)
- **Build**: PyInstaller (Windows), Tauri CLI (Linux)

## Status

**In Active Development**

Core architecture complete: launcher, backend/frontend scaffolding, NIM OAuth flow, Ollama integration, build pipelines for Windows/Linux. Conversation persistence, model switching, and desktop integration in progress.

## Repository Note

This repository contains documentation only. The implementation is private code.

## License

MIT