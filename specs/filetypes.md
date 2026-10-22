# File types

**Status:** Draft · **Prefix:** TYPE

## Purpose

Grouping files into categories by name, and showing how much space each type and each year takes.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| TYPE-1 | A category is `{name, colour, patterns[]}`; a pattern is a suffix pattern (`*.jpg`), an exact file name (`core`) or a wildcard (`*.so.*`), each with its own `case_sensitive` (default off). | category "images" with `*.jpg` → `A.JPG` matches |
| TYPE-2 | Matching order: exact names first, then the longest matching suffix, then wildcards in category order; the first hit wins; no hit → `other`. | `a.tar.gz`: `*.tar.gz` (archives) beats `*.gz` (compressed) |
| TYPE-3 | Default categories (editable, removable, re-addable from defaults): `junk`, `archives`, `compressed`, `images`, `raw_images`, `videos`, `music`, `documents`, `source_code`, `build_output`, `libraries`, `executables`, `disk_images`, `other`, each with a distinct colour and common suffixes. | fresh install → 14 categories listed |
| TYPE-4 | Type statistics for the current subtree: one row per category with file count, `total_size` and % of the subtree; each expands to its suffixes (or names) with the same columns; files with no suffix group as `no_extension`. Sorted by size, descending. | subtree with 3 `.png` (6 MiB) → images row 6 MiB, expands to `.png` |
| TYPE-5 | Suffixes not in any category are listed under `other` as their own rows (top 50 by size; the rest summed as one row). | 80 unknown suffixes → 50 rows + 1 rest row |
| TYPE-6 | Locate type: opening a suffix row lists every folder holding such files with their count and total size; opening a folder there selects those files in the tree ([TREE-8](tree.md)), ready for a cleanup. | open `.iso` row → folders listed; open one → its `.iso` files selected |
| TYPE-7 | Age statistics for the current subtree: one row per modified year with file count, total size and % of each; for the current and previous year, rows also per month. Opening a row lists those files ([DISC-1](discover.md)). | files from 2021 and 2024 → 2 year rows |
| TYPE-8 | Statistics recompute when their subtree changes (rescan, cleanup) and are not available while that subtree's scan is running. | delete a `.png` → images row drops |
| TYPE-9 | Categories are edited in settings: add, rename, recolour, reorder, edit patterns, delete (with confirmation when it has patterns); changes recolour the treemap at once. | recolour videos → map tiles change on save |

## Out of scope

- Detecting type by file content (magic bytes); OS content-type databases.
- Size distribution histograms and percentiles.

## Decisions

- Type by name only: reading content of millions of files would make the scan far slower, and most space hogs have telling suffixes.
- Category ids are generic; their display names are translated.
