# fheroes2 for Miyoo Mini Plus

Port of [fheroes2](https://github.com/ihhub/fheroes2) for the original **Miyoo Mini Plus**, targeting OnionOS 4.4 beta and a 640×480 display. This is an unofficial community build, not a port for Miyoo A30 or Flip.

## Download

See [Releases](https://github.com/Ivarge/fheroes2-miyoo-mini-plus/releases) for the installation ZIP, corresponding source ZIP, and SHA256 files.

Current build: **1.1.18 / miyoo-2026-11-24**. ARM build and regression checks passed. This specific release has not yet been tested on a physical console.

## Installation / Установка

1. Enable **Ports Collection** in Onion's Package Manager.
2. Extract `fheroes2-miyoo-mini-plus-onion-1.1.18.zip` into the root of the SD card, merging `Roms`.
3. Copy your own Heroes II game data into `Roms/PORTS/Games/FHeroes2/data` and maps into `maps`. The required file is `data/heroes2.agg`. Original commercial game data is not included.
4. Run **~import ports**, then **FHeroes2**.

При обновлении сохраните каталог `userdata`: в нём настройки и сохранения. Русский интерфейс выбирается в настройках игры; переводы включены. Разгон по умолчанию не применяется.

## Controls

D-pad moves the cursor; A is left click; B is right click (hold for information). X/Menu = Escape; Y/Start = Enter. L1 slows the cursor, R1 speeds it up. Select = next hero, L2 = hero window, R2 = end turn. Save and exit using the game's menus.

## Source and licensing

The engine and this port's own code are **GPL-2.0-or-later**; see [LICENSE](LICENSE). SDL and bundled third-party components retain their respective licenses.

**For the complete corresponding source, download `fheroes2-miyoo-mini-plus-1.1.18-source.zip` from the release assets.** GitHub's automatically generated “Source code” archives only contain this distribution repository's documents and are not the full engine source bundle.

The supplied source ZIP contains the modified engine, modified SDL, patches, build scripts, tests, license texts, and source/binary checksums. See [SOURCE-README-RU.md](SOURCE-README-RU.md) for reproduction instructions and dependency provenance. Publish the installation and matching source archive together.

Upstream engine revision: `9280fe5ee3af2cfc1036a0fa1c1e69eeb1b11f59` (1.1.18).
SDL fork baseline: `0631abc8e8916db6f9bc7e2afd0c22913d092a29` ([steward-fu/sdl2](https://github.com/steward-fu/sdl2)).

Miyoo fixes include complete ARGB8888 frame conversion/upload, full-panel MI_GFX output, bounded key-repeat handling, time-based cursor movement, and correctly encoded Russian translations.
