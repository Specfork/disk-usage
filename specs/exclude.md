# Exclude rules

**Status:** Draft · **Prefix:** EXCL

## Purpose

Skipping folders the user does not want scanned or counted.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| EXCL-1 | A rule is `{pattern, syntax: fixed\|wildcard\|regex, match: name\|full_path, case_sensitive, if_contains_file}`; defaults `wildcard`, `name`, case-sensitive on POSIX and off on Windows and macOS. | rule `node_modules` → every such folder excluded |
| EXCL-2 | Rules apply to folders only. A folder matching any rule is not read; it shows in the tree with state `excluded` ([SCAN-3](scan.md)), size 0, and the rule on hover. | `.git` rule → `.git` folders size 0, `excluded` |
| EXCL-3 | `if_contains_file`: the rule matches a folder when one of its direct files matches the pattern instead of the folder's own name (a marker such as `.nobackup`). | rule `.nobackup` with flag → folders holding that file excluded |
| EXCL-4 | Default rules: one, `.snapshot` (`fixed`, `name`); users may edit or remove it. | fresh install → 1 rule |
| EXCL-5 | "Exclude folder" in the item menu adds a `fixed`, `full_path` rule for it and marks it `excluded` at once, subtracting its size from all ancestors without a rescan. | exclude a 5 GiB folder → root total −5 GiB |
| EXCL-6 | Rule changes in settings apply to the next scan or rescan; an invalid regex cannot be saved. | save regex `(` → refused |
| EXCL-7 | A one-time `--exclude <pattern>` on the command line adds a `wildcard`, `name` rule for that run only. | run with `--exclude cache` → `cache` folders excluded, not saved |

## Out of scope

- Excluding individual files by pattern.

## Decisions

- Exclusions show as zero-size entries, not hidden ones, so the user sees why numbers are lower.
