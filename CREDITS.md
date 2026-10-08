# Credits

**Artwork:** Future cursors by yeyushengfan258 - <https://github.com/yeyushengfan258/Future-cursors>
(gnome-look: <https://www.gnome-look.org/p/1457141>; cyan: <https://www.gnome-look.org/p/1465392>). License: GPL-3.0. The repository's `LICENSE` is the GPL v3 text
with no "only" or "or later" notice anywhere; [LICENSE](LICENSE) here is that file, byte-for-byte
(SHA-256 `a0ee746064b06d09cab0768116ec265fd0d45261d4087c9ad2c698a07c7aac0e`).
**Based on:** [capitaine-cursors](https://github.com/keeferrourke/capitaine-cursors) by Keefer Rourke (LGPL-3.0),
as the upstream README states.
**Source:** `upstream/` is a git submodule pinned to commit `587c14d2f5bd2dc34095a4efbb1a729eb72a1d36` (2021-07-16,
the latest). The gnome-look download has the same artwork with the hotspots from before upstream's last two commits
(which only move hotspots).
**Windows 11 HiDPI port:** Vadym Herman ([@hervad](https://github.com/hervad)), built with
[w11-cursor-toolkit](https://github.com/hervad/w11-cursor-toolkit)

## Changes from upstream

- Re-rendered from the original SVGs (`src/svg/*.svg` for Amber, `src/svg-cyan/*.svg` for Cyan; 32 px canvas) at
  every Windows cursor size, no resampling. Both variants ship from this one repo: same artwork, only the accent
  colour differs (all 93 drawings render with identical shapes).
- Drop shadow left out: upstream draws it as blurred dark copies of the shapes (elements with an SVG filter).
  Rendered with and without them, no artwork pixel changes in any shipped file. Windows draws its own pointer
  shadow (toolkit ADR-14).
- Hotspots as in upstream's `src/config/*.cursor` (32 px line), except:
  - Help (4,4) -> (6,4): the same arrow as Normal Select, which upstream moved to (6,4) in its hotspot fix; (4,4) is
    2 px left of the tip, outside the drawing.
  - Alternate Select (16,4) -> (16,8): the arrow's tip is at y = 8; (16,4) is 4 px above it.
- Animation: 23 frames as upstream; 30 ms per frame approximated in whole 1/60 s steps (683 ms per cycle instead of
  690 ms). The busy animation's last frame is empty in upstream too.
- Help badge: `overrides/default/help.svg` and `overrides/cyan/help.svg` are upstream's `help.svg` with one element
  removed - a white disc exactly under the badge's dark ring (same centre, same radius 6.5). It was hidden on purpose
  but anti-aliasing let it show as a light halo around the ring once the shadow was gone (upstream's own Inkscape
  build shows no halo). Removing it changes only the ring's outer edge pixels.
- Windows role mapping incl. Pin and Person (both use the pointing hand).
