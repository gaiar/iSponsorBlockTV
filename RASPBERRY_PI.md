# Raspberry Pi Support

This fork provides enhanced Docker image support for all Raspberry Pi models through automated multi-architecture builds.

## Supported Architectures

| Architecture | Raspberry Pi Models | Docker Platform |
|:-------------|:-------------------|:----------------|
| ARMv6        | Pi Zero, Pi 1      | `linux/arm/v6`  |
| ARMv7        | Pi 2, Pi 3         | `linux/arm/v7`  |
| ARM64        | Pi 4, Pi 5         | `linux/arm64`   |

## Quick Start

### Using Docker Compose

```yaml
version: '3.8'
services:
  isponsorblocktv:
    image: ghcr.io/gaiar/isponsorblocktv:latest
    container_name: isponsorblocktv
    volumes:
      - ./config:/app/data
    restart: unless-stopped
    network_mode: host
```

### Using Docker Run

```bash
# Create config directory
mkdir -p ~/isponsorblocktv/config

# Run container (will automatically pull the correct architecture)
docker run -d \
  --name isponsorblocktv \
  --network host \
  --restart unless-stopped \
  -v ~/isponsorblocktv/config:/app/data \
  ghcr.io/gaiar/isponsorblocktv:latest
```

## Installation by Raspberry Pi Model

### Raspberry Pi Zero / Pi 1 (ARMv6)
```bash
# Verify architecture
uname -m  # Should show: armv6l

# Pull specific architecture (optional, Docker will auto-select)
docker pull --platform linux/arm/v6 ghcr.io/gaiar/isponsorblocktv:latest
```

### Raspberry Pi 2 / Pi 3 (ARMv7)
```bash
# Verify architecture  
uname -m  # Should show: armv7l

# Pull specific architecture (optional, Docker will auto-select)
docker pull --platform linux/arm/v7 ghcr.io/gaiar/isponsorblocktv:latest
```

### Raspberry Pi 4 / Pi 5 (ARM64)
```bash
# Verify architecture
uname -m  # Should show: aarch64

# Pull specific architecture (optional, Docker will auto-select)
docker pull --platform linux/arm64 ghcr.io/gaiar/isponsorblocktv:latest
```

## Automated Updates

This fork automatically syncs with the upstream repository ([dmunozv04/iSponsorBlockTV](https://github.com/dmunozv04/iSponsorBlockTV)) every hour and rebuilds Docker images when changes are detected.

## Performance Notes

- **Pi Zero/1**: May experience slower performance due to ARMv6 architecture
- **Pi 2/3**: Good performance for most use cases
- **Pi 4/5**: Excellent performance, recommended for heavy usage

## Troubleshooting

### Check Current Architecture
```bash
docker run --rm ghcr.io/gaiar/isponsorblocktv:latest uname -m
```

### Force Architecture Pull
```bash
# Force pull specific architecture if auto-detection fails
docker pull --platform linux/arm/v6 ghcr.io/gaiar/isponsorblocktv:latest  # Pi Zero/1
docker pull --platform linux/arm/v7 ghcr.io/gaiar/isponsorblocktv:latest  # Pi 2/3  
docker pull --platform linux/arm64 ghcr.io/gaiar/isponsorblocktv:latest   # Pi 4/5
```

### Memory Considerations
For Pi Zero/1, consider adding memory limits:
```yaml
services:
  isponsorblocktv:
    # ... other config
    mem_limit: 256m
    memswap_limit: 512m
```