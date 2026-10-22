# Cleanup

**Status:** Draft · **Prefix:** CLN

## Purpose

Acting on what the scan found (trash, delete, or any command) safely, then updating the numbers.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| CLN-1 | A cleanup action is `{id, title, kind: builtin\|command, command?, enabled, works_for: [file, folder, loose_files], destructive, confirm, refresh, recurse, output, shell, shortcut?}`; it appears in menus only for selections where every item's kind is in `works_for`. | action for folders only → hidden when a file is selected |
| CLN-2 | Built-in actions, which can be disabled but not removed: `open_in_file_manager`, `open_terminal_here`, `move_to_trash` (Delete), `delete_permanently` (Shift+Delete), and `compress_folder` (archives the folder beside itself, then removes it after success; marked `destructive`). | fresh install → 5 built-in actions |
| CLN-3 | `move_to_trash` and `delete_permanently` always confirm, whatever `confirm` says: the dialog lists the first 20 paths, the item count, and the summed `total_size` without double-counting nested items; the default button is Cancel. `delete_permanently` adds that it cannot be undone. | Shift+Delete on 3 items → dialog "3 items, 1.2 GiB", Cancel focused |
| CLN-4 | Protected paths are never trashed, deleted or passed to a command action marked `destructive`: the OS root, the user's home folder itself, any mount point, and the scan root itself. The action is refused with a message naming the path. | delete on a mount point → refused |
| CLN-5 | If the trash is unavailable for an item's volume, `move_to_trash` stops and offers `delete_permanently` with its own confirmation (CLN-3) for those items only. | volume without trash → second confirmation |
| CLN-6 | Command actions expand placeholders before running: `{path}` (full path), `{name}` (last component), `{dir}` (the folder itself, or a file's parent), `{terminal}` and `{file_manager}` (the platform's default apps); every substituted value is quoted for the chosen shell. | name `a b'c.txt` → passed as one argument |
| CLN-7 | Commands run with the working folder set to `{dir}`; `shell` is `user_default` (default), a named shell, or `none` (run directly). With several items the command runs once per item, in tree order, one at a time. | 3 files selected → 3 runs |
| CLN-8 | `confirm` (default off for commands, on for those marked `destructive`) shows the expanded command line(s) and item count before running. | destructive action → expanded command shown first |
| CLN-9 | `recurse` runs the command in the folder and in each of its subfolders, deepest first. | recurse on `a/` with `a/b/` → runs in `a/b`, then `a` |
| CLN-10 | `refresh` after the action: `none`, `this` (rescan the item), `parent` (rescan its parent), `assume_deleted` (remove it from the tree without rescanning). Built-in trash and delete use `assume_deleted` only for items that are really gone, and rescan the parent otherwise. | trash file → gone from tree, parent totals drop |
| CLN-11 | `output`: `always`, `on_error` (default; opens when the command writes to stderr or exits non-zero), `after_delay` (opens if still running after 5 s), `never`. The output view shows each command line, stdout, stderr (visually distinct) and exit code, and can stop the running command. | failing command → output opens with exit code |
| CLN-12 | Cleanups are disabled while a scan of the affected subtree runs, and for scans opened from a file ([EXP-3](export.md)) until rescanned from disk. | during scan → cleanup menu disabled |
| CLN-13 | User actions are added, edited, reordered, enabled or disabled, and removed in settings; removing one asks for confirmation. | remove action → confirm → gone from menus |

## Out of scope

- Undo of permanent deletion; secure erase.
- Scheduled or automatic cleanups.

## Decisions

- Trash and delete confirm always (CLN-3); only user commands may skip confirmation, and only when not marked `destructive`.
- Protected paths (CLN-4) are refused outright rather than double-confirmed: deleting a mount point or a home folder from a disk-usage view is never what the user meant.
- Placeholder names are generic (`{path}`); the source uses its own macro syntax.
