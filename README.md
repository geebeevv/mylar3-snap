# Mylar3 Snap

Unofficial snap package for [Mylar3](https://github.com/mylar3/mylar3) - an automated Comic Book (CBR/CBZ) downloader for NZB and torrents.

## About

This is an **UNOFFICIAL** snap package of the Mylar3 application.

Mylar3 is an automated Comic Book (cbr/cbz) downloader program for use with NZB and torrents. It supports SABnzbd, NZBGet, and many torrent clients in addition to DDL. It will allow you to monitor weekly pull-lists for items belonging to user-watchlists, and generate download requests based on that.

Official project: https://github.com/mylar3/mylar3

## Why Use the Snap?

While Mylar3 is easy to install manually, the snap offers several advantages:

- **Zero dependency management** - No need to manually install Python packages or worry about conflicts
- **Automatic updates** - Get new versions without manual intervention
- **Auto-starts on boot** - Runs as a system service with no configuration needed
- **Easy management** - Simple CLI commands for common tasks (status, logs, backup, etc.)
- **Configuration via snap set** - Change settings without editing config files
- **Built-in backup/restore** - Protect your data with one command
- **Isolated environment** - Doesn't interfere with other Python applications
- **Consistent across distributions** - Same experience on Ubuntu, Fedora, Arch, etc.
- **Rollback support** - Revert to previous versions if needed

## Installation

### Stable Channel (Recommended)

```bash
sudo snap install geebeevv-mylar3
```

The service will autostart automatically on installation.

### Beta Channel (Early Testing)

Help test new releases before they reach stable:

```bash
sudo snap install geebeevv-mylar3 --beta
```

Beta builds are automatically created when upstream releases new versions and go through testing before promotion to stable.

## Configuration

Configuration and database files are stored in `/var/snap/geebeevv-mylar3/common/data/`

Access the web interface at: http://localhost:8090

## File Access

Mylar3 needs access to your download directories and comic library. By default, snaps have restricted file system access.

### Grant Access to External Directories

Connect the removable-media interface to allow Mylar3 to access external drives and directories:

```bash
sudo snap connect geebeevv-mylar3:removable-media
```

This allows access to `/media`, `/mnt`, and `/run/media` directories.

## Easy Management with CLI Commands

The snap includes convenient commands for managing Mylar3:

```bash
# View service status and configuration
geebeevv-mylar3.status

# View logs (last 100 lines)
geebeevv-mylar3.logs
geebeevv-mylar3.logs -f  # Follow mode

# Show configuration directory
geebeevv-mylar3.config-dir

# Check service health
geebeevv-mylar3.health-check

# Backup configuration and database
geebeevv-mylar3.backup

# Restore from backup
geebeevv-mylar3.restore <timestamp>
```

## Configuration via snap set

Customize Mylar3 behavior without editing config files:

```bash
# Change web interface port (default: 8090)
sudo snap set geebeevv-mylar3 port=8091

# Enable verbose logging (default: false)
sudo snap set geebeevv-mylar3 verbose=true

# Enable automatic backups on startup (options: none, ini, db, both)
sudo snap set geebeevv-mylar3 auto-backup=both

# Disable weekly pull list check on startup (default: false)
sudo snap set geebeevv-mylar3 disable-weekly-check=true

# View current configuration
geebeevv-mylar3.status
```

The service automatically restarts when configuration changes.

## Manual Control

If you prefer traditional snap commands:

```bash
sudo snap start geebeevv-mylar3
sudo snap stop geebeevv-mylar3
sudo snap restart geebeevv-mylar3
sudo snap logs geebeevv-mylar3
```

## Important Notes

- This snap runs as a service with root privileges
- Data persists across snap updates in the common directory
- Comic library path should be accessible (use removable-media interface)
- Download client integration works via network API calls

## For Maintainers

This repository uses an automated workflow to track upstream releases. When Mylar3 releases a new version:

1. GitHub Actions automatically detects it
2. Updates the `beta` branch and triggers a Snapcraft build
3. Creates a Pull Request with release notes and testing checklist
4. After testing and approval, merging promotes to stable channel

See [WORKFLOW.md](WORKFLOW.md) for detailed documentation.

## License

See [LICENSE](LICENSE) file.
