# Discover

**Status:** Draft · **Prefix:** DISC

## Purpose

Ready-made searches that surface the files most worth a look, plus a name search.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| DISC-1 | Every search runs over the current item's subtree (the root if none) and opens a results list with columns `size`, `modified`, `path`, sortable, with a count; opening a row selects it in the tree and treemap; cleanup actions work on selected rows. | open row → item selected in tree |
| DISC-2 | Largest files: the 200 files with the largest `size`, descending. | subtree of 1,000 files → 200 rows, first = biggest |
| DISC-3 | Newest files: the 200 most recently modified files; oldest files: the 200 least recently modified. | newest → first row is latest `modified` |
| DISC-4 | Hard-linked files: every file with links > 1, with its link count as an extra column. | file with 3 links → listed, links 3 |
| DISC-5 | Broken links: every symbolic link whose target does not exist, with the target as an extra column. | link to deleted file → listed |
| DISC-6 | Sparse files and offline files ([SIZE-4](sizes.md), [SIZE-5](sizes.md)): one search each, with `bytes` and `allocated` as extra columns. | sparse file → listed with both sizes |
| DISC-7 | Files from a year or month (from age statistics, [TYPE-7](filetypes.md)): every file whose modified time falls in it. | 2021 row → only 2021 files |
| DISC-8 | Find (Ctrl+F): a text with mode `contains` (default), `starts_with`, `ends_with`, `exact`, `wildcard` or `regex`; `case_sensitive` (default off); type filter `files`, `folders`, `links`, `all` (default `files`). An invalid regex is refused with a message. | `ends_with` `.log` → only names ending `.log` |
| DISC-9 | Find returns at most 1,000 results and then says the list was cut at 1,000. | 5,000 matches → 1,000 + notice |
| DISC-10 | Result lists drop rows for items removed by a cleanup or rescan without being reopened. | trash a listed file → row disappears |

## Out of scope

- Duplicate-file finding by content.
- Saved searches.

## Decisions

- Fixed caps (200, 1,000) keep result lists usable; a narrower subtree gives more precise results.
