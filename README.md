# iSponsorBlockTV - Enhanced Multi-Architecture Fork

[![Docker Build](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/build_docker_images.yml?logo=docker&label=docker%20build)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/build_docker_images.yml)
[![Multi-Arch](https://img.shields.io/badge/docker-multi--arch-blue?logo=docker)](https://github.com/gaiar/iSponsorBlockTV#architecture-support)
[![Upstream Sync](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/sync-upstream.yml?logo=github&label=upstream%20sync)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/sync-upstream.yml)

Enhanced fork of [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV) with extended architecture support and automated upstream synchronization.

iSponsorBlockTV is a self-hosted application that connects to your YouTube TV app and automatically skips segments (like Sponsors or intros) in YouTube videos using the [SponsorBlock](https://sponsor.ajay.app/) API. It can also auto mute and press the "Skip Ad" button the moment it becomes available on YouTube ads.

## Fork Enhancements

| Feature | Original | This Fork |
|:--------|:---------|:----------|
| **Docker Architectures** | 3 (amd64, arm64, armv7) | 5 (amd64, i386, arm64, armv7, armv6) |
| **Raspberry Pi Support** | Pi 2, 3, 4 | All Models (Zero, 1, 2, 3, 4, 5) |
| **Legacy x86 Support** | No | 32-bit i386 systems |
| **Container Registry** | DockerHub + GHCR | GHCR (unlimited pulls) |
| **Upstream Synchronization** | Manual | Automated hourly sync |
| **Documentation** | Basic | Complete Docker guide |

## Docker Installation

### Docker Run
```bash
# Create config directory
mkdir -p ~/isponsorblocktv/config

# Run container
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

Save as `docker-compose.yml` and run:
```bash
docker compose up -d
```

## Architecture Support

Supported Docker platforms:

| Architecture | Platform | Example Systems |
|:-------------|:---------|:----------------|
| amd64 | `linux/amd64` | Intel/AMD PCs, Servers, NAS |
| i386 | `linux/i386` | 32-bit PCs, Legacy Systems |
| arm64 | `linux/arm64` | RPi 4/5, Apple Silicon, ARM Servers |
| armv7 | `linux/arm/v7` | RPi 2/3, Many ARM Devices |
| armv6 | `linux/arm/v6` | RPi Zero/1, Older ARM Devices |

Docker automatically selects the correct architecture.

## Raspberry Pi Support

All Raspberry Pi models supported:

| Pi Model | Architecture |
|:---------|:-------------|
| Pi 5 | ARM64 |
| Pi 4 | ARM64 |
| Pi 3 | ARMv7 |
| Pi 2 | ARMv7 |
| Pi Zero/1 | ARMv6 |

For detailed setup instructions, see [RASPBERRY_PI.md](RASPBERRY_PI.md).

Memory limits for older models:
```yaml
# docker-compose.yml for Pi Zero/1
services:
  isponsorblocktv:
    mem_limit: 256m
    memswap_limit: 512m
```

## Container Images

Available registries:
- `ghcr.io/gaiar/isponsorblocktv:latest`
- `ghcr.io/gaiar/isponsorblocktv:develop`
- `ghcr.io/gaiar/isponsorblocktv:v2.6.0`

GitHub Container Registry provides unlimited pulls and better performance compared to DockerHub rate limits.

## Automatic Upstream Synchronization

This fork automatically synchronizes with the original repository:
- Hourly checks for new commits from [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV)
- Auto-merge upstream changes when detected
- Auto-rebuild Docker images with latest code
- Conflict detection with graceful handling

## Device Compatibility

Same device support as the original:

| Device | Status | Device | Status |
|:-------|:------:|:-------|:------:|
| Apple TV | ✅ | Roku | ✅ |
| Samsung TV (Tizen) | ✅ | Fire TV | ✅ |
| LG TV (WebOS) | ✅ | CCwGTV | ✅ |
| Android TV | ✅ | Nintendo Switch | ✅ |
| Chromecast | ✅ | Xbox One/Series | ✅ |
| Google TV | ✅ | Playstation 4/5 | ✅ |

Legend: ✅ = Working, ❌ = Not working, ❔ = Not tested

## Usage

1. Start the container using Docker or Docker Compose
2. Configure devices:
   - Auto-discovery: Ensure your computer is on the same network as your TV device during initial setup
   - Manual setup: Get the YouTube TV code from your device's settings and add it to iSponsorBlockTV
3. Access web interface for configuration
4. Network requirements: Only requires access to youtube.com (doesn't need to be on same network as TV after setup)

For detailed setup instructions, check the [original wiki](https://github.com/dmunozv04/iSponsorBlockTV/wiki/Installation).

## Advanced Usage

Force specific architecture:
```bash
docker pull --platform linux/arm/v6 ghcr.io/gaiar/isponsorblocktv:latest  # Pi Zero/1
docker pull --platform linux/arm/v7 ghcr.io/gaiar/isponsorblocktv:latest  # Pi 2/3
docker pull --platform linux/arm64 ghcr.io/gaiar/isponsorblocktv:latest   # Pi 4/5
docker pull --platform linux/i386 ghcr.io/gaiar/isponsorblocktv:latest    # 32-bit x86
```

Verify architecture:
```bash
docker run --rm ghcr.io/gaiar/isponsorblocktv:latest uname -m
```

## Libraries Used

- [pyytlounge](https://github.com/FabioGNR/pyytlounge) - Device interaction
- [aiohttp](https://github.com/aio-libs/aiohttp) - Async HTTP client/server
- [async-cache](https://github.com/iamsinghrajat/async-cache) - Async caching
- [Textual](https://github.com/textualize/textual/) - Terminal UI framework
- [ssdp](https://github.com/codingjoe/ssdp) - Auto-discovery protocol

## Projects Using This Project

- [Home Assistant Addon](https://github.com/bertybuttface/addons/tree/main/isponsorblocktv) - Enhanced with multi-arch support

## Contributing

This fork welcomes contributions for:
- Architecture-specific optimizations
- Raspberry Pi compatibility improvements
- Documentation enhancements
- Bug reports for specific device types

For core functionality, contribute to the [original repository](https://github.com/dmunozv04/iSponsorBlockTV) - changes will be automatically synchronized to this fork.

For fork-specific features:
1. Fork this repository
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Attribution

This is an enhanced fork of [iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV) by [dmunozv04](https://github.com/dmunozv04). All core functionality and device compatibility comes from the original project.

[![Contributors](https://contrib.rocks/image?repo=dmunozv04/iSponsorBlockTV)](https://github.com/dmunozv04/iSponsorBlockTV/graphs/contributors)

## License

[![GNU GPLv3](https://www.gnu.org/graphics/gplv3-127x51.png)](https://www.gnu.org/licenses/gpl-3.0.en.html)

GNU General Public License v3.0