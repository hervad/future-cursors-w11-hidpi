# Port status: Future cursors

- gnome-look pages: https://www.gnome-look.org/p/1457141 (Future) and https://www.gnome-look.org/p/1465392
  (Future-cyan) - store API 2026-10-08: one download each, licence field empty on both
- upstream: https://github.com/yeyushengfan258/Future-cursors, submodule `upstream/` pinned to
  `587c14d2f5bd2dc34095a4efbb1a729eb72a1d36` (2021-07-16, latest commit)
- upstream license: GPL-3.0. `LICENSE` = GPL v3 text (SHA-256 `a0ee7460…7aac0e`), copied byte-for-byte. No
  "only"/"or later" notice. README: "based on capitaine-cursors" (LGPL-3.0, Keefer Rourke) -> credited.
- variants: default = amber (`src/svg`, page p/1457141) and cyan (`src/svg-cyan`, page p/1465392) - one repo for
  both (maintainer, 2026-10-08; ADR-9 amendment). `svg-black` / `svg-dark` have their own pages (p/1519633,
  p/1457884) and are not in scope.

## Status
Public at https://github.com/hervad/future-cursors-w11-hidpi (2026-10-08), CI on toolkit v0.2.0: validate OK, Windows
loader 34/34. Released v0.1.0 (2026-10-08).
Future-cyan merged in (2026-10-08): no separate future-cyan-cursors-w11-hidpi repo.

## Findings
- **Canvas:** all 93 SVGs `viewBox="0 0 32 32"` (29 of them `32.000001` high) -> `design_canvas = 32`; upstream
  build.sh renders 1x at 32 px.
- **Store download vs repo** (2026-10-08): the gnome-look tarball holds compiled Xcursors only (no SVG, no licence).
  Pixels are identical to the repo's `dist/`; 12 files differ only in hotspot (upstream's last two commits,
  f55048a/3994eeb "hotzones"), 40 "differences" are git symlink stubs on Windows. The pin includes the fixes.
- **Shadow:** 69 of 93 SVGs use filters, often on groups and as `filter=` attributes (structure differs from Material).
  Render check (resvg, 128 px, with vs without filtered elements): in all 118 shipped files (svg + svg-cyan) 0 pixels
  change where the remaining art is opaque -> only an under-shadow is removed. (Files that are NOT shipped -
  bottom_side, cell, dnd-no-drop - do lose art pixels; irrelevant here, noted for future role changes.)
- **Hotspots** (`src/config/*.cursor`, "x1" line = 32 px; the 40/48/64 px lines are exact point scalings):
  kept as upstream except help (4,4) -> (6,4) (outside the drawing; same arrow as default, which upstream moved to
  (6,4)) and up-arrow (16,4) -> (16,8) (art starts at y = 8.0). Centred cursors are (16,16) upstream = true centre.
- **Animation:** wait/progress, 23 frames x 30 ms (all 92 config lines) -> 41 jiffies = 683 ms per cycle (690).
  busy is a build-up/fade of hexagons: visible px at 32 px rise 78 -> 454 -> 28, frame 23 is EMPTY (as upstream) ->
  the cursor vanishes for ~17 ms per cycle. Kept faithful.
- **Sizes:** working.ani 468 KB, busy.ani 221 KB; largest image offset 11,985 of 65,535.
- **Load cost** (Windows 11 25H2, same run as aero): static 0.16-0.26 ms at 32-96 px (aero 0.12-0.14), animated
  2.2-3.5 ms (aero 1.2-4.1), 31 ms at 256 px (aero 19); 0 GDI/USER handles leaked over 300 loads.
- **Cyan = recolour** (2026-10-08): svg-cyan has the same 93 file names; 25 byte-identical, 55 colour-only
  (#ffba00/#ffb900 -> #23afc8), 13 re-saved by Inkscape (metadata/serialisation). Rendered at 128 px all 93 have
  identical alpha (0 differing px). The store's cyan tarball (compiled Xcursor only) has accent exactly #23afc8 and
  the same 4 sizes as upstream's build -> it is svg-cyan.
- **Help halo** (maintainer spotted, 2026-10-08): white circle (21.5,23.5) r 6.5 sits exactly under the dark badge
  ring -> light anti-aliasing fringe ("disconnected ring") on dark backgrounds: 48 light px in the band outside the
  ring at 96 px; 0 in upstream's store build (Inkscape + baked shadow). Fixed with overrides/{default,cyan}/help.svg
  (that element removed; 126 edge px change). Scan of every shipped source in Polar, Material and Future (remove each
  shape, keep those visible only as a thin line): help was the only light-under-dark halo; the other hits are real
  outlines (unavailable rings) or interior seams (Material busy petals).
- **Existing Windows port:** chiyuki0325/Future-cyan-cursors-Windows (7a5ee50, 2023-01-04), inspected read-only in
  a temp folder (nothing copied): 15 files, each one 32 px BMP; wait 24 frames / progress 22 frames at 2 jiffies
  (800 / 733 ms per cycle); hotspots (7,5) on not-allowed and the animations. None found for amber Future.

## Checklist
- [x] Upstream pinned (submodule at a commit); latest commit confirmed
- [x] License verified by reading the actual LICENSE file -> ./LICENSE (byte-identical); capitaine credited
- [x] design_canvas confirmed from SVG viewBox (32)
- [x] All 17 roles mapped; diagonals in the preview; Pin/Person = link
- [x] Hotspots from upstream config; two corrected with evidence
- [x] `w11cursor build` + `validate` green locally; Test-LoadCursors 34/34; Get-AniFrameTiming 4/4
- [x] README, CREDITS, preview image
- [x] Installed on Windows 11 25H2 (2026-10-08): both variants, loader 34/34 on C:\Windows\Cursors, live Arrow/Help =
      installed files (Help = the halo-free override); maintainer checked on screen: "looks ok"
- [x] GitHub repo created (public); CI green
- [x] tag v0.1.0 (2026-10-08)
