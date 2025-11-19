# Mylar3 Snap

**Unofficial** snap package for [Mylar3](https://github.com/mylar3/mylar3).

## Description

Mylar3 is an automated Comic Book (cbr/cbz) downloader for NZB and torrent sources. Supports SABnzbd, NZBGet, qBittorrent, and other download clients. Monitors pull-lists and manages comic library.

Official project: https://github.com/mylar3/mylar3

## Known Issues

- Runs as daemon with root privileges
- Limited file system access due to snap confinement (requires manual interface connections)

## Installation

```bash
# Stable channel
sudo snap install geebeevv-mylar3

# Beta channel (testing)
sudo snap install geebeevv-mylar3 --beta
```

Service auto-starts on installation. Web interface: http://localhost:8090

Data directory: `/var/snap/geebeevv-mylar3/common/data/`

## Post-Install Interface Connections

Required for file system access beyond snap confinement:

```bash
# Access to /media, /mnt, /run/media
sudo snap connect geebeevv-mylar3:removable-media

# Mount table access
sudo snap connect geebeevv-mylar3:mount-observe
```

## CLI Commands

```bash
geebeevv-mylar3.status         # Service status and configuration
geebeevv-mylar3.logs           # View logs (last 100 lines)
geebeevv-mylar3.logs -f        # Follow logs
geebeevv-mylar3.config-dir     # Show config directory path
geebeevv-mylar3.health-check   # Service health verification
geebeevv-mylar3.backup         # Backup config and database
geebeevv-mylar3.restore <ts>   # Restore from backup timestamp
```

## Configuration Options

Runtime configuration via `snap set`:

```bash
# Port (default: 8090)
sudo snap set geebeevv-mylar3 port=8091

# Verbose logging (default: false)
sudo snap set geebeevv-mylar3 verbose=true

# Auto-backup on startup (options: none, ini, db, both)
sudo snap set geebeevv-mylar3 auto-backup=both

# Disable weekly pull check (default: false)
sudo snap set geebeevv-mylar3 disable-weekly-check=true
```

Service restarts automatically when configuration changes.

## Service Control

```bash
sudo snap start geebeevv-mylar3
sudo snap stop geebeevv-mylar3
sudo snap restart geebeevv-mylar3
```

## Maintainer Notes

Automated upstream release tracking:

1. GitHub Actions detects new Mylar3 versions
2. Updates `beta` branch and triggers Snapcraft build
3. Creates PR with release notes and testing checklist
4. Merge to main promotes to stable channel

See [WORKFLOW.md](WORKFLOW.md) for details.

## License

See [LICENSE](LICENSE) file.
