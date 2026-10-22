# Size tree

**Status:** Draft · **Prefix:** TREE

## Purpose

The main list: the scanned hierarchy with sizes, sorted so the biggest things come first.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| TREE-1 | The tree shows the scan root and its descendants, expandable per folder; the root starts expanded one level. | scan → root's children visible |
| TREE-2 | Columns: `name` (always first), `percent_bar`, `percent`, `size`, `allocated`, `items`, `files`, `folders`, `latest_mtime`, `oldest_mtime`, `owner`, `group`, `permissions`. The user can hide, show and reorder all but `name`; the layout persists. | hide `owner`, restart → still hidden |
| TREE-3 | When a folder has both files and subfolders, its direct non-folder items are grouped under a pseudo-child `loose_files` (shown as "files" in the user's language) with its own roll-ups, so they compare against subfolders; a folder with only files lists them directly. | folder with 2 subfolders + 30 files → 3 children |
| TREE-4 | Default sort: `size` descending at every level; clicking any column sorts by it, again reverses; ties break by name. Sorting applies to all levels and is kept across rescans. | sort by `latest_mtime` → newest-changed first |
| TREE-5 | `percent_bar` draws `percent` as a bar whose colour depends on tree depth, so nested levels are told apart. | expand 3 levels → 3 bar colours |
| TREE-6 | Items with state `error`, `aborted`, `excluded`, `not_crossed`, `duplicate`, or flags `sparse`, `offline`, show a distinct mark and the reason on hover. | unreadable folder → warning mark, hover "permission denied" |
| TREE-7 | Selection: click selects one, Ctrl+click toggles, Shift+click selects a range; the status line shows the count and summed `total_size` of selected items, without double-counting items inside selected folders. | select folder + a file inside it → sum = folder size |
| TREE-8 | Selection and current item sync with the treemap both ways ([MAP-7](treemap.md)); selecting from the treemap expands the tree down to the item and scrolls to it. | click tile → tree expands to that file |
| TREE-9 | Navigation: go up one level (Alt+Up), go to the root (Alt+Home), a breadcrumb path for the current item with clickable parts, and "expand to level" 0–9 that opens every folder down to that depth and closes the rest. | expand to level 2 → depth ≤ 2 open |
| TREE-10 | Copy path (Ctrl+C) puts the full paths of the selected items on the clipboard, one per line; "open containing folder" shows the item in the OS file manager. | select 2 → 2 lines |
| TREE-11 | The context menu on items offers: rescan ([EXP-5](export.md)), cleanup actions that apply ([CLN-1](cleanup.md)), exclude folder ([EXCL-5](exclude.md)), copy path, continue scanning here (for `not_crossed`), details. | right-click mount point → "continue scanning here" |
| TREE-12 | Expanding a folder of 100,000 direct children shows it within 1 s and sorting it within 1 s. | manual: generated folder of 100,000 files |

## Out of scope

- Bookmarks of favourite folders.
- Editing names or moving items by drag and drop.

## Decisions

- Grouping loose files (TREE-3) is shown in both tree and treemap so a folder's own files are one comparable block.
