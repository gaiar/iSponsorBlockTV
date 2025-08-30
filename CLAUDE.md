# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Context

This is an enhanced fork of dmunozv04/iSponsorBlockTV with extended multi-architecture Docker support and automated upstream synchronization. The core application remains unchanged - this fork focuses on containerization and deployment enhancements.

## Architecture Overview

### Application Structure
- **Python 3.9+ application** using asyncio and aiohttp for async operations
- **Entry points**: `src/main.py` (CLI) and `src/main_tui.py` (TUI)
- **Main package**: `src/iSponsorBlockTV/` contains core application logic
- **Key modules**:
  - `main.py`: Core application logic and event handling
  - `ytlounge.py`: YouTube TV communication via pyytlounge library
  - `dial_client.py`: Device discovery using DIAL protocol
  - `setup_wizard.py`: Interactive configuration using Textual TUI
  - `config_setup.py`: Configuration file management

### Containerization
- **Multi-stage Dockerfile**: Optimized build with bytecode compilation and dependency separation
- **Multi-architecture builds**: Supports 5 architectures (amd64, i386, arm64, armv7, armv6)
- **GitHub Container Registry**: Primary registry at `ghcr.io/gaiar/isponsorblocktv`

## Development Commands

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Run application (CLI mode)
python src/main.py

# Run application (TUI mode)  
python src/main_tui.py

# Install as package
pip install -e .

# Run installed package
iSponsorBlockTV
```

### Docker Development
```bash
# Build multi-arch image locally
docker buildx build --platform linux/amd64,linux/arm64 -t isponsorblocktv .

# Test specific architecture
docker buildx build --platform linux/arm/v6 -t isponsorblocktv:armv6 .
docker run --rm isponsorblocktv:armv6 uname -m

# Run container with config mount
docker run -d --name isponsorblocktv --network host \
  -v ./config:/app/data ghcr.io/gaiar/isponsorblocktv:latest
```

### Code Quality
```bash
# Lint with ruff (configured for 100 char line length)
ruff check .

# Format with ruff
ruff format .
```

## Fork-Specific Features

### Automated Upstream Synchronization
- **Workflow**: `.github/workflows/sync-upstream.yml`
- **Schedule**: Hourly checks for upstream changes
- **Process**: Auto-merge → Auto-build → Auto-publish
- **Conflict handling**: Creates GitHub issue if merge conflicts occur

### Enhanced Multi-Architecture Builds
- **Workflow**: `.github/workflows/build_docker_images.yml` 
- **Architectures**: Extends original 3 to 5 architectures
- **Added support**: linux/i386 (32-bit x86), linux/arm/v6 (RPi Zero/1)
- **Registry**: Publishes to GHCR only (no DockerHub rate limits)

### Architecture-Specific Notes
- **ARMv6 builds**: May require longer build times due to QEMU emulation
- **i386 builds**: Support for legacy 32-bit x86 systems
- **Build caching**: Uses GitHub Actions cache for faster builds

## Workflow Management

### Triggering Builds
```bash
# Manual workflow trigger
gh workflow run build_docker_images.yml

# Manual upstream sync
gh workflow run sync-upstream.yml

# Check workflow status
gh run list --limit 5
```

### Image Verification
```bash
# Check multi-arch manifest
docker manifest inspect ghcr.io/gaiar/isponsorblocktv:latest

# Verify architecture availability
docker manifest inspect ghcr.io/gaiar/isponsorblocktv:latest | \
  jq -r '.manifests[] | select(.platform) | "\(.platform.architecture)/\(.platform.variant // "none")"'
```

## Important Files

- **RASPBERRY_PI.md**: Detailed setup instructions for all Pi models
- **pyproject.toml**: Package configuration and ruff settings (100 char line length)
- **requirements.txt**: Runtime dependencies (aiohttp, textual, pyytlounge, etc.)
- **Dockerfile**: Multi-stage build with bytecode compilation
- **.github/workflows/**: Fork-specific automation (sync + build)

## Relationship to Upstream

This fork automatically synchronizes with the original repository and should only contain containerization/deployment enhancements. Core application changes should be contributed to the upstream repository at dmunozv04/iSponsorBlockTV.