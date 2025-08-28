# iSponsorBlockTV - Enhanced Multi-Architecture Fork

[![Docker Build](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/build_docker_images.yml?logo=docker&label=docker%20build)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/build_docker_images.yml)
[![Multi-Arch](https://img.shields.io/badge/docker-multi--arch-blue?logo=docker)](https://github.com/gaiar/iSponsorBlockTV#architecture-support)
[![Upstream Sync](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/sync-upstream.yml?logo=github&label=upstream%20sync)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/sync-upstream.yml)

Enhanced fork of [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV) with extended architecture support and automated upstream synchronization.

## Fork Enhancements

| Feature | Original | This Fork |
|:--------|:---------|:----------|
| Docker Architectures | 3 (amd64, arm64, armv7) | 5 (amd64, i386, arm64, armv7, armv6) |
| Raspberry Pi Support | Pi 2, 3, 4 | All Models (Zero, 1, 2, 3, 4, 5) |
| Legacy x86 Support | No | 32-bit i386 systems |
| Container Registry | DockerHub + GHCR | GHCR (unlimited pulls) |
| Upstream Synchronization | Manual | Automated hourly sync |

## Docker Installation

### Docker Run
```bash
mkdir -p ~/isponsorblocktv/config

docker run -d \
  --name isponsorblocktv \
  --network host \
  --restart unless-stopped \
  -v ~/isponsorblocktv/config:/app/data \
  ghcr.io/gaiar/isponsorblocktv:latest
```

### Docker Compose
```yaml
version: '3.8'
services:
  isponsorblocktv:
    image: ghcr.io/gaiar/isponsorblocktv:latest
    container_name: isponsorblocktv
    network_mode: host
    restart: unless-stopped
    volumes:
      - ./config:/app/data
```

## Architecture Support

| Architecture | Platform | Example Systems |
|:-------------|:---------|:----------------|
| amd64 | `linux/amd64` | Intel/AMD PCs, Servers, NAS |
| i386 | `linux/i386` | 32-bit PCs, Legacy Systems |
| arm64 | `linux/arm64` | RPi 4/5, Apple Silicon, ARM Servers |
| armv7 | `linux/arm/v7` | RPi 2/3, Many ARM Devices |
| armv6 | `linux/arm/v6` | RPi Zero/1, Older ARM Devices |

## Raspberry Pi Support

| Pi Model | Architecture |
|:---------|:-------------|
| Pi 5 | ARM64 |
| Pi 4 | ARM64 |
| Pi 3 | ARMv7 |
| Pi 2 | ARMv7 |
| Pi Zero/1 | ARMv6 |

For detailed setup instructions, see [RASPBERRY_PI.md](RASPBERRY_PI.md).

## Container Images

Available from GitHub Container Registry:
- `ghcr.io/gaiar/isponsorblocktv:latest`
- `ghcr.io/gaiar/isponsorblocktv:develop`

## Automatic Upstream Synchronization

This fork automatically synchronizes with the original repository:
- Hourly checks for new commits from [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV)
- Auto-merge upstream changes when detected
- Auto-rebuild Docker images with latest code

## About the Original Project

For device compatibility, usage instructions, and core functionality details, see the [original repository](https://github.com/dmunozv04/iSponsorBlockTV).

## Attribution

Enhanced fork of [iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV) by [dmunozv04](https://github.com/dmunozv04). All core functionality comes from the original project.

## License

GNU General Public License v3.0