# Rescan and export

**Status:** Draft · **Prefix:** EXP

## Purpose

Refreshing parts of a scan, and saving scans to look at later or on another machine.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| EXP-1 | Scan file: gzip-compressed JSON Lines. Line 1: `{format: "disk-usage-scan", version: 1, root, scanned_at, os, options: {cross_filesystems, split_hard_links, excludes[]}}`; then one line per item in depth-first order: `{path, type, bytes, allocated, links, mtime, state?, flags?, owner?, group?, mode?}` with `path` relative to `root` and `mtime` UTC ISO 8601. | save, gunzip → line 1 has `format` and `version` |
| EXP-2 | "Save scan" (Ctrl+S) writes the current tree, or a selected subtree, to a file the user picks (default name `<root name>-<YYYY-MM-DD>.scan.gz`); folders still being read are saved with state `aborted`. | save during scan → `aborted` folders in file |
| EXP-3 | "Open scan" loads a scan file into the tree, treemap and statistics as if scanned; the title shows the file name and `scanned_at`. Cleanups stay disabled ([CLN-12](cleanup.md)) until a rescan from disk. | open file → tree shows, delete disabled |
| EXP-4 | A file with another `format`, a newer major `version`, or a corrupt line is refused with the line number; nothing partial is shown. | truncated file → refused, "line 1234" |
| EXP-5 | Rescan the selected folder (Ctrl+R) or everything (F5): reads that subtree afresh, replaces it in place, updates ancestors' roll-ups, and keeps expansion and selection for paths that still exist. | rescan after adding 1 GiB → folder +1 GiB, still expanded |
| EXP-6 | A rescan of a folder that no longer exists removes it from the tree and updates ancestors. | delete folder outside the app, rescan it → removed |
| EXP-7 | Any results or statistics list exports as CSV (UTF-8, comma, header row, sizes in bytes, times UTC ISO 8601). | export largest files → CSV with 200 rows + header |
| EXP-8 | Saving and reopening a scan of 1,000,000 items each take ≤ 10 s and the file is ≤ 60 MiB. | manual: 1 M-item scan |

## Out of scope

- Reading other tools' scan formats; a separate headless scanner program.
- Automatic use of scan files found inside scanned folders.

## Decisions

- JSON Lines in gzip: stream-friendly, readable by any stack, and greppable after decompressing; the source uses its own line-based text format.
- Opened scans are read-only for cleanups: their paths may not exist on this machine or may have changed.
