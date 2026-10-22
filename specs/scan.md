# Scan

**Status:** Draft · **Prefix:** SCAN

## Purpose

Reading a folder or drive into an in-memory tree, fast, visibly and safely.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SCAN-1 | A scan starts from a folder chosen in a picker, typed as a path, passed on the command line, picked from recent folders (last 10), or from the volumes overview (SCAN-11). A path that does not exist or is not a folder is refused with a message. | `/nope` → refused, no scan |
| SCAN-2 | The scan walks every entry below the root without following symbolic links or junctions; each entry gets a type: `file`, `dir`, `symlink`, `block_device`, `char_device`, `fifo`, `socket`, `other`. | link to `/usr` inside root → listed as `symlink`, not entered |
| SCAN-3 | Each folder has a read state: `queued`, `reading`, `finished`, `aborted` (scan stopped), `error` (could not be listed), `excluded` ([EXCL](exclude.md)), `not_crossed` (mount point not entered, SCAN-7), `duplicate` (SCAN-9). A folder's subtree counts as complete only when all its folders are `finished`, `excluded`, `not_crossed` or `duplicate`. | stop mid-scan → some folders `aborted`, root incomplete |
| SCAN-4 | Results appear while the scan runs: the tree and totals update at least every `update_interval_ms` (default 500); the treemap is drawn when the scan finishes ([MAP-1](treemap.md)). | manual: large scan shows growing totals |
| SCAN-5 | Status shows elapsed time, folders read, items found and the folder being read; Stop (Esc) ends the scan within 1 s and keeps everything read so far. | Esc → scan ends, partial tree kept |
| SCAN-6 | A folder that cannot be listed (permission, I/O error) gets state `error`, keeps size 0, shows a warning mark, and is counted in a "N folders unreadable" notice that opens a list of their paths and reasons. | `chmod 000` folder → `error`, notice "1 folder unreadable" |
| SCAN-7 | `cross_filesystems` (default off): a mount point below the root on another volume is shown with state `not_crossed`, size 0, and a mount mark; "continue scanning here" scans just that one. | external drive mounted under root → `not_crossed` |
| SCAN-8 | Even with `cross_filesystems` on, virtual or system filesystems (process, device, kernel tables) and network filesystems are not entered unless scanned explicitly; sub-volumes of the root's own volume (same device) are entered as part of it. | cross on, `/proc` below root → `not_crossed` |
| SCAN-9 | A folder reached twice through bind mounts, firmlinks or similar (same device and file id as a folder already read) is scanned once; later paths get state `duplicate` and size 0. | bind-mount `a` at `b` inside root → `b` `duplicate` |
| SCAN-10 | Reading uses at most `scan_threads` (default 4) concurrent folder reads and keeps the interface responsive throughout. | manual: scroll tree during a scan, no freeze |
| SCAN-11 | Volumes overview: every mounted volume with mount path, device or label, filesystem type, total, used, reserved, free and free %; virtual and system mounts hidden by default with a toggle; opening a row scans it. | open overview → system disk listed with free % |
| SCAN-12 | Scanning 1,000,000 files on a local SSD finishes within 2 × the time the OS's own disk-usage command needs on a warm cache, using ≤ 300 MiB of memory for the tree. | manual: 1 M-file tree, compare times, read memory |

## Out of scope

- Network-transparent scanning of remote hosts (scan files made elsewhere open via [EXP](export.md)).
- Package-manager views (which files belong to installed packages).
- Live watching for changes after the scan.

## Decisions

- Mount points are not crossed by default so a drive's numbers are not inflated by other volumes mounted inside it.
- Duplicate folders (SCAN-9) count once; the source handles bind mounts by mount table only, this rule covers firmlinks and junction-like cases on every OS.
