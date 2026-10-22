# Treemap

**Status:** Draft · **Prefix:** MAP

## Purpose

A picture of the scanned tree where every file is a rectangle whose area is its size.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| MAP-1 | The treemap shows the subtree of the current zoom root (default the scan root); it is built when a scan or rescan finishes and rebuilt after cleanups, zoom and window resizes (debounced 200 ms). During a scan it shows a progress placeholder. | resize window → map redraws once after resize ends |
| MAP-2 | Layout is squarified: each folder's area is split among its children in size order so tiles stay close to square; tile area is proportional to `total_size` ([SIZE-6](sizes.md)). | folder of 1 GiB + 3 GiB children → tiles 1 : 3 by area |
| MAP-3 | Only files (and `loose_files` groups, [TREE-3](tree.md)) are painted as tiles; folders show as nesting, with optional 1-px borders when `show_folder_borders` (default off). Items of size 0 get no tile. | empty file → no tile |
| MAP-4 | A tile whose longer side is under `min_tile_px` (default 3) is not drawn; its area is painted in its parent's colour. | 100,000 tiny files → no sub-3 px tiles |
| MAP-5 | Tile colour = the file's category colour ([TYPE-1](filetypes.md)); uncategorised files use the `other` colour. | `.mp4` tile in the videos colour |
| MAP-6 | Shading: `cushion` (default; each tile lit like a soft bump so tile edges are visible) or `flat` (solid colours with 1-px gaps). | switch to flat → no gradients |
| MAP-7 | Click selects a tile and makes it current; Ctrl+click toggles it in the selection; the current item has a strong outline, other selected items a lighter one; changes sync to the tree ([TREE-8](tree.md)). | Ctrl+click 2 tiles → both outlined, both selected in tree |
| MAP-8 | Hovering a tile shows its path and size in the status line within 100 ms. | manual: hover |
| MAP-9 | Highlight ancestors: Shift+click (or middle click) outlines every ancestor folder of the clicked tile and dims the rest of the map; the same action again clears it. | Shift+click file → its folders outlined |
| MAP-10 | Zoom in (Ctrl+Plus, double-click, wheel up) makes the folder one level below the zoom root on the path to the current item the new zoom root; zoom out (Ctrl+Minus, wheel down) goes up one level; reset (Ctrl+0) returns to the scan root. | double-click a deep tile 3 times → 3 levels in |
| MAP-11 | Show or hide the treemap (F9); the split between tree and treemap is draggable and persists. | F9 → map hidden; restart → still hidden |
| MAP-12 | Building the map for 1,000,000 files on a 1920 × 1080 area takes ≤ 1 s on a mid-range laptop. | manual: time redraw after resize |

## Out of scope

- 3-D or radial (sunburst) charts; exporting the map as an image.

## Decisions

- The map draws after the scan, not during, so constantly moving tiles never distract or cost time.
