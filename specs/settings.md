# Settings

**Status:** Draft · **Prefix:** SET

## Purpose

Preferences, their defaults, keyboard shortcuts and the command line.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| SET-1 | Settings live in one local file per user and apply at once; defaults: `cross_filesystems` off, `split_hard_links` on, `update_interval_ms` 500, `scan_threads` 4, `show_folder_borders` off, `min_tile_px` 3, `shading` `cushion`, `treemap_visible` on, `recent_folders` (last 10), tree columns and sort ([TREE-2](tree.md), [TREE-4](tree.md)), categories ([TYPE-3](filetypes.md)), exclude rules ([EXCL-4](exclude.md)), cleanup actions ([CLN-2](cleanup.md)). | delete settings file → app starts with these defaults |
| SET-2 | "Reset to defaults" per settings page restores that page's defaults after a confirmation. | reset cleanups → user actions removed after confirm |
| SET-3 | Window size, position, split position and open statistics windows are restored on next start. | resize, restart → same size |
| SET-4 | Default shortcuts: open folder Ctrl+O, rescan all F5, rescan selected Ctrl+R, stop Esc, find Ctrl+F, copy path Ctrl+C, trash Delete, delete permanently Shift+Delete, treemap toggle F9, zoom Ctrl+Plus / Ctrl+Minus / Ctrl+0, go up Alt+Up, save scan Ctrl+S; on macOS Cmd replaces Ctrl. User cleanup actions may get their own shortcut; a clash with an existing one is refused. | assign F5 to an action → refused |
| SET-5 | Command line: `<app> [folder]` scans it; `--open <scan-file>` opens a saved scan; `--exclude <pattern>` ([EXCL-7](exclude.md)); `--cross-filesystems`; `--version`; an unknown option prints usage and exits with code 2. | `--bogus` → usage, exit 2 |
| SET-6 | "Clear recent folders" empties the list at once. | clear → list empty |
| SET-7 | The app needs no account, works fully offline and sends nothing over the network: no update checks, analytics or crash uploads. | manual: full session with network blocked, watch traffic |
| SET-8 | Interface text is translatable; numbers, dates and sizes follow the OS locale, keeping binary units ([SIZE-9](sizes.md)). | German locale → `34,4 MiB` |

## Out of scope

- Themes beyond following the OS light/dark setting.
- Sync of settings between machines.

## Decisions

- No `@specfork/privacy` requirement: the app stores no personal records, only settings, recent folders (SET-6 clears them) and scan files the user saves deliberately.
