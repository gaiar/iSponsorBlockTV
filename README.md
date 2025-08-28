# iSponsorBlockTV - Enhanced Multi-Architecture Fork

[![Docker Build](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/build_docker_images.yml?logo=docker&label=docker%20build)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/build_docker_images.yml)
[![Multi-Arch](https://img.shields.io/badge/docker-multi--arch-blue?logo=docker)](https://github.com/gaiar/iSponsorBlockTV#architecture-support)
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-All%20Models-c51a4a?logo=raspberry-pi)](https://github.com/gaiar/iSponsorBlockTV#raspberry-pi-support)
[![Upstream Sync](https://img.shields.io/github/actions/workflow/status/gaiar/iSponsorBlockTV/sync-upstream.yml?logo=github&label=upstream%20sync)](https://github.com/gaiar/iSponsorBlockTV/actions/workflows/sync-upstream.yml)
[![GitHub Release](https://img.shields.io/github/v/release/dmunozv04/isponsorblocktv?logo=GitHub&style=flat&label=upstream%20version)](https://github.com/dmunozv04/iSponsorBlockTV/releases/latest)

**Enhanced fork of [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV)** with complete multi-architecture support and automated upstream synchronization.

iSponsorBlockTV is a self-hosted application that connects to your YouTube TV app and automatically skips segments (like Sponsors or intros) in YouTube videos using the [SponsorBlock](https://sponsor.ajay.app/) API. It can also auto mute and press the "Skip Ad" button the moment it becomes available on YouTube ads.

## ✨ What's Enhanced in This Fork

| Feature | Original | This Fork |
|:--------|:---------|:----------|
| **Docker Architectures** | 3 (amd64, arm64, armv7) | **5** (amd64, i386, arm64, armv7, armv6) |
| **Raspberry Pi Support** | Pi 2, 3, 4 | **All Models** (Zero, 1, 2, 3, 4, 5) |
| **Legacy x86 Support** | ❌ | ✅ **32-bit i386 systems** |
| **Container Registry** | DockerHub + GHCR | **GHCR (unlimited pulls, better performance)** |
| **Upstream Synchronization** | Manual | **Automated hourly sync** |
| **Documentation** | Basic | **Complete Docker guide + Pi-specific instructions** |

## 🚀 Quick Start (Docker)

### Option 1: Docker Run (Simplest)
```bash
# Create config directory
mkdir -p ~/isponsorblocktv/config

# Run container (automatically pulls correct architecture)
docker run -d \
  --name isponsorblocktv \
  --network host \
  --restart unless-stopped \
  -v ~/isponsorblocktv/config:/app/data \
  ghcr.io/gaiar/isponsorblocktv:latest
```

### Option 2: Docker Compose (Recommended)
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

## 🏗️ Architecture Support

This fork provides Docker images for **all modern systems**:

| Architecture | Platform | Example Systems | Performance |
|:-------------|:---------|:----------------|:------------|
| **amd64** | `linux/amd64` | Intel/AMD PCs, Servers, NAS | ⭐⭐⭐⭐⭐ Excellent |
| **i386** | `linux/i386` | 32-bit PCs, Legacy Systems | ⭐⭐⭐⭐ Good |
| **arm64** | `linux/arm64` | **RPi 4/5**, Apple Silicon, ARM Servers | ⭐⭐⭐⭐⭐ Excellent |
| **armv7** | `linux/arm/v7` | **RPi 2/3**, Many ARM Devices | ⭐⭐⭐⭐ Good |
| **armv6** | `linux/arm/v6` | **RPi Zero/1**, Older ARM Devices | ⭐⭐⭐ Basic |

Docker automatically selects the correct architecture for your system.

## 🍓 Raspberry Pi Support

**All Raspberry Pi models supported** with optimized Docker images:

| Pi Model | Architecture | Docker Command Example |
|:---------|:-------------|:------------------------|
| **Pi 5** | ARM64 | `docker run ghcr.io/gaiar/isponsorblocktv:latest` |
| **Pi 4** | ARM64 | `docker run ghcr.io/gaiar/isponsorblocktv:latest` |
| **Pi 3** | ARMv7 | `docker run ghcr.io/gaiar/isponsorblocktv:latest` |
| **Pi 2** | ARMv7 | `docker run ghcr.io/gaiar/isponsorblocktv:latest` |
| **Pi Zero/1** | ARMv6 | `docker run ghcr.io/gaiar/isponsorblocktv:latest` |

For detailed Pi-specific setup instructions, see [RASPBERRY_PI.md](RASPBERRY_PI.md).

### Performance Tips for Older Pi Models
```yaml
# For Pi Zero/1: Add memory limits to docker-compose.yml
services:
  isponsorblocktv:
    # ... other config ...
    mem_limit: 256m
    memswap_limit: 512m
```

## 📦 Container Images

**Primary Registry (Recommended):**
- **Latest**: `ghcr.io/gaiar/isponsorblocktv:latest`
- **Development**: `ghcr.io/gaiar/isponsorblocktv:develop`
- **Tagged versions**: `ghcr.io/gaiar/isponsorblocktv:v2.6.0`

**Why GitHub Container Registry?**
- ✅ **Unlimited pulls** for public images
- ✅ **No rate limits** (unlike DockerHub's 100 pulls/6hrs)
- ✅ **Better performance** and reliability
- ✅ **Multi-architecture manifest** support

## 🔄 Automatic Upstream Synchronization

This fork automatically stays synchronized with the original repository:
- 🕐 **Hourly checks** for new commits from [dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV)
- 🔄 **Auto-merge** upstream changes when detected
- 🚀 **Auto-rebuild** Docker images with latest code
- 🛡️ **Conflict detection** with graceful handling

You always get the latest features from the original project with enhanced architecture support!

## 🎯 Device Compatibility

Same excellent device support as the original:

| Device | Status | Device | Status |
|:-------|:------:|:-------|:------:|
| Apple TV | ✅ | Roku | ✅ |
| Samsung TV (Tizen) | ✅ | Fire TV | ✅ |
| LG TV (WebOS) | ✅ | CCwGTV | ✅ |
| Android TV | ✅ | Nintendo Switch | ✅ |
| Chromecast | ✅ | Xbox One/Series | ✅ |
| Google TV | ✅ | Playstation 4/5 | ✅ |

Legend: ✅ = Working, ❌ = Not working, ❔ = Not tested

## 💻 Usage

1. **Start the container** using one of the quick start methods above
2. **Configure devices**: 
   - **Auto-discovery**: Ensure your computer is on the same network as your TV device during initial setup
   - **Manual setup**: Get the YouTube TV code from your device's settings and add it to iSponsorBlockTV
3. **Access web interface**: The application provides a web interface for configuration
4. **Network requirements**: Only requires access to youtube.com (doesn't need to be on same network as TV after setup)

For detailed setup instructions, check the [original wiki](https://github.com/dmunozv04/iSponsorBlockTV/wiki/Installation).

## 🔧 Advanced Usage

### Force Specific Architecture
```bash
# Force pull specific architecture (usually not needed)
docker pull --platform linux/arm/v6 ghcr.io/gaiar/isponsorblocktv:latest  # Pi Zero/1
docker pull --platform linux/arm/v7 ghcr.io/gaiar/isponsorblocktv:latest  # Pi 2/3
docker pull --platform linux/arm64 ghcr.io/gaiar/isponsorblocktv:latest   # Pi 4/5
docker pull --platform linux/i386 ghcr.io/gaiar/isponsorblocktv:latest    # 32-bit x86
```

### Verify Current Architecture
```bash
docker run --rm ghcr.io/gaiar/isponsorblocktv:latest uname -m
```

## 🛠️ Libraries Used

- [pyytlounge](https://github.com/FabioGNR/pyytlounge) - Device interaction
- [aiohttp](https://github.com/aio-libs/aiohttp) - Async HTTP client/server
- [async-cache](https://github.com/iamsinghrajat/async-cache) - Async caching
- [Textual](https://github.com/textualize/textual/) - Terminal UI framework
- [ssdp](https://github.com/codingjoe/ssdp) - Auto-discovery protocol

## 🏠 Projects Using This Project

- [Home Assistant Addon](https://github.com/bertybuttface/addons/tree/main/isponsorblocktv) - Enhanced with multi-arch support

## 🤝 Contributing

This fork welcomes contributions, especially:
- **Architecture-specific optimizations**
- **Raspberry Pi compatibility improvements** 
- **Documentation enhancements**
- **Bug reports** for specific device types

### For the Original Project
To contribute to the core functionality, please contribute to the [original repository](https://github.com/dmunozv04/iSponsorBlockTV) - changes will be automatically synchronized to this fork.

### For Fork-Specific Features
1. Fork this repository
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## 📝 Attribution

This is an enhanced fork of the excellent [iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV) by [dmunozv04](https://github.com/dmunozv04). All core functionality and device compatibility comes from the original project.

**Original Contributors:**
[![Contributors](https://contrib.rocks/image?repo=dmunozv04/iSponsorBlockTV)](https://github.com/dmunozv04/iSponsorBlockTV/graphs/contributors)

Made with [contrib.rocks](https://contrib.rocks).

## 📄 License

[![GNU GPLv3](https://www.gnu.org/graphics/gplv3-127x51.png)](https://www.gnu.org/licenses/gpl-3.0.en.html)

Same license as the original project: GNU General Public License v3.0