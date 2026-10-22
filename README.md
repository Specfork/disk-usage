---
specfork: 1
name: "@specfork/disk-usage"
summary: Desktop disk usage analyzer for Windows, macOS and Linux. Scan a folder or drive, explore a size tree and treemap, see file types and largest files, and clean up safely.
license: CC-BY-4.0
derived_from:
  repo: https://github.com/shundhammer/qdirstat
  commit: ab3f28460a61264b7d7bc2fc356a0851e8736f26
  url: https://github.com/shundhammer/qdirstat
  license: GPL-2.0-only
---

# Disk usage

A desktop program that shows where disk space goes. It scans a folder or drive, sums sizes up a
tree, draws a treemap where area equals size, groups files by type, finds the largest, newest,
oldest and oddest files, and runs cleanup actions (trash, delete, custom commands) with
confirmation and an automatic rescan. Scans can be saved and reopened elsewhere. Behaviour
derived from QDirStat's open-source code (GPL-2.0-only), written fresh and stack-free.
Not affiliated with or endorsed by Stefan Hundhammer. The name is used only to credit the source.

| Area | Prefix | Covers |
|---|---|---|
| [scan.md](specs/scan.md) | SCAN | Starting, progress, stopping, item types, read states, mount boundaries, volumes overview, performance |
| [sizes.md](specs/sizes.md) | SIZE | Size model: apparent and allocated size, hard links, sparse and offline files, roll-ups, units |
| [tree.md](specs/tree.md) | TREE | Size tree: columns, loose-files group, sorting, percent bars, selection, navigation |
| [treemap.md](specs/treemap.md) | MAP | Treemap layout, colours, shading, zoom, selection sync, performance |
| [filetypes.md](specs/filetypes.md) | TYPE | File categories, type statistics, locating files of a type, age statistics |
| [discover.md](specs/discover.md) | DISC | Largest, newest, oldest, hard-linked, broken-link and sparse files; find by name |
| [cleanup.md](specs/cleanup.md) | CLN | Cleanup actions, placeholders, confirmation, protected paths, refresh policies, output |
| [exclude.md](specs/exclude.md) | EXCL | Exclude rules for folders |
| [export.md](specs/export.md) | EXP | Saving and reopening scans, list export |
| [settings.md](specs/settings.md) | SET | Preferences, defaults, keyboard shortcuts, offline guarantee |

Single user, local only: no account, no network, no telemetry. Every rule runs on one device.
Sizes use binary units (1 KiB = 1024 B); times are local in the interface and UTC ISO 8601 in files;
enum values are `snake_case`. Keyboard shortcuts are defaults; a build may follow its platform's
conventions instead.
