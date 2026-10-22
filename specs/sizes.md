# Sizes

**Status:** Draft · **Prefix:** SIZE

## Purpose

What "size" means for each item and folder, so numbers match real disk use.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SIZE-1 | Each item has `bytes` (apparent length from the filesystem) and `allocated` (space the filesystem reports as used for it). Folders and links count their own entry's size too. | 1-byte file on 4 KiB-block disk → bytes 1, allocated 4 KiB |
| SIZE-2 | An item's `size` (the default measure everywhere) is `bytes`, except for sparse files (SIZE-4), whose `size` is `allocated`. | normal 10 MiB file → size 10 MiB |
| SIZE-3 | A file with `links` > 1 hard links counts `size / links` and `allocated / links` at each path, so the paths together count it once, whether or not they are all inside the scan. With `split_hard_links` (default on) turned off, every path counts the full size. | 1 GiB file, 2 links, both scanned → total 1 GiB |
| SIZE-4 | A file is sparse when `allocated` + 64 KiB < `bytes`; it is flagged `sparse`. | 1 GiB file with 1 MiB written → sparse, size ≈ 1 MiB |
| SIZE-5 | Files whose content is not stored locally (cloud placeholders, offline files) are flagged `offline` and have `allocated` 0 and `size` 0; their `bytes` still shows. | 2 GiB cloud-only file → size 0, `offline` |
| SIZE-6 | Folder roll-ups: `total_size`, `total_allocated`, `total_items`, `total_files`, `total_folders` sum the subtree (including the folder's own entry); `latest_mtime` is the newest modified time in the subtree, `oldest_mtime` the oldest file time. | folder with files from 2019 and 2024 → oldest 2019, latest 2024 |
| SIZE-7 | `percent` of an item = its `total_size` ÷ its parent's `total_size` × 100, one decimal; a parent of total 0 gives children blank percent. | 3 MiB in 4 MiB parent → 75.0 |
| SIZE-8 | Roll-ups update incrementally during the scan, after rescans and after cleanups; a folder whose subtree is incomplete ([SCAN-3](scan.md)) shows its totals with a `≥` mark. | stop scan → partial folders show `≥` |
| SIZE-9 | Sizes display with binary units `B`, `KiB`, `MiB`, `GiB`, `TiB`, `PiB`, three significant digits (`34.4 MiB`); the exact byte count shows on hover and in item details. | 36,116,381 B → `34.4 MiB`, hover shows exact |
| SIZE-10 | Item details for the current item: path, type, size, bytes, allocated, links, `sparse`/`offline` flags, modified time, owner and group and permissions where the OS has them (blank otherwise), and for folders the roll-ups of SIZE-6. | select a file → details show all fields |

## Out of scope

- Filesystem-level compression or deduplication savings, snapshots and copy-on-write sharing; these explain why scan totals and a volume's "used" can differ, which the volumes overview ([SCAN-11](scan.md)) helps compare.

## Decisions

- Hard links are split per link (SIZE-3) so each path's share is stable whatever the scan root; the source does the same.
- The sparse threshold of 64 KiB avoids flagging ordinary files whose last block is partly used.
- `offline` is added for cloud-synced folders on Windows and macOS, which the source does not handle.
