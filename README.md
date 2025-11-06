# Mylar3 Snap

Unofficial snap package for [Mylar3](https://github.com/mylar3/mylar3) - an automated Comic Book (CBR/CBZ) downloader for NZB and torrents.

## About

This is an **UNOFFICIAL** snap package of the Mylar3 application.

Mylar3 is an automated Comic Book (cbr/cbz) downloader program for use with NZB and torrents. It supports SABnzbd, NZBGet, and many torrent clients in addition to DDL. It will allow you to monitor weekly pull-lists for items belonging to user-watchlists, and generate download requests based on that.

Official project: https://github.com/mylar3/mylar3

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

## Manual Control

```bash
sudo snap start geebeevv-mylar3
sudo snap stop geebeevv-mylar3
sudo snap restart geebeevv-mylar3
```

## Viewing Logs

```bash
sudo snap logs geebeevv-mylar3
sudo snap logs geebeevv-mylar3 -f  # Follow mode
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
