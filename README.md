# CommandByVoice - 1750 Colonial Americas MOD

A turn-based grand-strategy content mod for **CommandByVoice** set in the
mid-18th-century Americas. The map is a hand-drawn simplification of the
Atlantic world as it stood in 1750 - just before the Seven Years' War
overturned the European overseas order - and the campaign revolves around
twelve rival powers: ten European colonial projects and two large blocs of
Indigenous nations, each led by a colonial Governor or a tribal chief. The
player inherits one of those leaders and is asked to expand, develop, and
defend their colony; to weather demands from European overlords; and to
navigate the plausible period politics between empires, borderland colonies,
and the Indigenous nations whose lands they sit on.

## Highlights

- **12 playable countries** (10 European colonial administrations + 2
  Indigenous-nation blocs)
- **45 hand-drawn Voronoi provinces** over a simplified 1750 Americas map
- **Hand-authored map geometry** (`Maps/DefaultVoronoiMap.json`, SHA-256
  locked to `69c00dcd...6f873c`)
- **AI-generated officer / concubine / flag portraits** generated
  locally with Stable Diffusion XL via the KoboldCpp A1111-compatible
  shim
- **Hand-curated country roster, official portraits and biographical
  blurbs** drawn from 1750 colonial Americas history

## Map SHA-256 (locked)

```
Maps/DefaultVoronoiMap.json   69c00dcd8de6597fb5f0c708d9892183ab2eec063ba747a908aba9cd976f873c
```

If your local checkout of `DefaultVoronoiMap.json` hashes to anything
else, the map has been edited; do **not** file issue reports against this
mod until you restore the locked geometry.

## Country roster

| ID    | Country / Faction                        | Culture                                       | Leader (defaults)              |
| ----- | ---------------------------------------- | --------------------------------------------- | ------------------------------ |
| 200001 | British America                          | British 1750s colonial administration         | William Pitt (Governor)        |
| 200002 | Captaincy General of Cuba / Floridas    | Spanish colonial viceregal administration     | Diego de Peza y Valle          |
| 200003 | Nouvelle-France                          | French colonial administration of New France  | Louis-Joseph de Montcalm       |
| 200004 | Viceroyalty of New Spain                 | Spanish colonial viceregal administration     | Juan Francisco de Revillagigedo |
| 200005 | Lakota & Northeastern Woodlands nations | Indigenous nations                            | (tribal council)               |
| 200006 | Cheyenne / Crow / Ute                    | Plains Indigenous nations                     | Atakullakulla                  |
| 200007 | Chinook / Shoshone                       | Pacific Northwest Indigenous nations          | Comcomly                       |
| 200008 | Viceroyalty of Peru                      | Spanish viceregal + Andean administration     | Teodoro de Croix               |
| 200009 | Luso-Brazilian America                   | Portuguese colonial administration of Brazil  | Marquês de Pombal              |
| 200010 | Mapuche & Andean confederations         | Mapuche toqui / Andean confederations         | (tribal council)               |
| 200011 | Viceroyalty of New Granada               | Spanish colonial viceregal administration     | Pedro de Bourbon               |
| 200012 | Wichí / Guaraní / Charrúa                | Chacoan Indigenous nations                    | (tribal council)               |

The four European colonial blocs that *faced each other directly* in 1750 -
British North America, New France, New Spain, and Captaincy-General Cuba -
are each given a full complement of named era-appropriate officers (Pitt /
Pelham / Clive / Wolfe / Amherst; Montcalm / Bougainville / Lévis;
Revillagigedo; Peza y Valle) so that the Seven Years' War feels like the
turning point it was.

## Installation (Steam Proton / Linux)

1. Locate your CommandByVoice `LocalLow` folder used by the game running
   under Proton. On Linux with Steam Play it is typically:
   ```
   ~/.steam/steam/steamapps/compatdata/<CommandByVoice-appid>/pfx/drive_c/users/steamuser/AppData/LocalLow/Xideimeicai/CommandByVoice/
   ```
   A symmetric Proton path used in this repo is:
   ```
   /path/to/proton-shared-home/steamuser/AppData/LocalLow/Xideimeicai/CommandByVoice/
   ```
2. Inside that folder, find or create `Mods/Local/`.
3. Copy or clone this entire directory into `Mods/Local/` as
   ```
   Mods/Local/mod_20260810055229_949bff/
   ```
   so that the layout becomes
   ```
   Mods/Local/mod_20260810055229_949bff/
     manifest.json
     Maps/
       DefaultVoronoiMap.json
     Data/
       Data_Country.json
       ...
     Art/
     Prompts/
     Skills/
     preview.png
     LICENSE
     README.md
     .gitignore
   ```
4. Launch CommandByVoice and select the mod from the in-game **MODs**
   menu; the entry scenario id is `mod_20260810055229_949bff`.

### Windows / native Steam

The same layout applies - drop the folder under
`%LOCALAPPDATA%Low\Xideimeicai\CommandByVoice\Mods\Local\` (Windows) or
`~/Library/Application Support/Xideimeicai/CommandByVoice/Mods/Local/`
(macOS).

## Compatibility

- **Game**: CommandByVoice `gameVersion` `1.0.0`,
  `contentSchemaVersion` `1`
- **Steam / Proton**: tested on Proton 9.x through 10.x; should work on any
  Proton version that successfully launches the base game.
- **Disk footprint**: ~70 MB total (the bulk of which is the Voronoi map
  geometry under `Maps/`).
- **Dependencies**: none - this mod is fully self-contained. The
  `Prompts/` and `Skills/` directories are authoring artifacts (Stable
  Diffusion prompt logs + a sketch style guide used during portrait
  generation) and are not consumed by the game at runtime.

## License

Released under **CC0 1.0 Universal (Public Domain Dedication)**.
See [`LICENSE`](./LICENSE) for the full legal text. You may copy, modify,
sell, fork, redistribute, or use any part of this mod in commercial or
non-commercial works without attribution - though a link back is always
appreciated.

The base game **CommandByVoice**, the Steam client, and Proton remain
under their own respective licenses and are not part of this dedication.

## Credits

- **Map geometry** (`Maps/DefaultVoronoiMap.json`) is **hand-drawn** by the
  project owner; do not claim authorship of it.
- **Officer portraits, concubine portraits, and country flags** were
  generated locally with **Stable Diffusion XL** running through
  **KoboldCpp's A1111-compatible API shim**. See `Prompts/` for the
  generation prompts that produced them and `Skills/historical-ink-portrait/`
  for the sketch / ink-portrait style guide that was iterated on.
- **Country names, faction rosters, and period-appropriate officers**
  were selected from 1750 colonial Americas history; see the standard
  reference works on the Seven Years' War and the Bourbon / Habsburg
  overseas reforms.

## Known limitations

- **No music modding support.** The game's official modding surface does
  not currently expose per-mod music slots; the upstream engine treats
  the soundtrack as a single global asset. Until that is fixed, this mod
  ships without a custom soundtrack and uses the vanilla game's music.
  Tracking issue: see the engine-side report in the parent project.
- **Province expansion is hard-capped at 45.** Going beyond that would
  require the in-game **map editor** mode, which is currently disabled in
  shipped builds. 45 provinces is the maximum Voronoi-cell count this map
  can sustain with stable cell sizes for the 1750 Americas extent.
- **18 of the 45 provinces are inactive** by design:
  - **ProvinceIDs 12, 15, 18** are reserved as geographic placeholders
    (open Atlantic, mid-Pacific, Patagonia margin) and intentionally have
    no Voronoi cells.
  - **ProvinceID 42** (Antarctic reach) is also intentionally empty - 0
    Voronoi cells.
  These four IDs are the only `unused` IDs in `Data/Data_ProvinceConfig.json`
  and the engine treats them as inert slots. Leave them in place.

## Contributing

Contributions, ports to other era-specific Americas maps, additional
officer rosters, and bug reports are welcome. The project will be
mirrored publicly for the first time at the GitHub repository URL TBD
(placeholder - will be filled in once the user publishes this repo);
please file issues and pull requests there.

When contributing:

- Do not edit `Maps/DefaultVoronoiMap.json` outside of an explicit
  "map geometry change" PR - the SHA-256 is locked and downstream users
  detect drift.
- Add officers / concubines through `Prompts/canonical-roster.json`,
  then regenerate the corresponding `Data_Data_DefaultOfficials.json` and
  `Data_Concubine.json` rows.
- Re-run the post-build SHA baseline after every change that touches a
  tracked `Data/*.json` (`Prompts/post-build-shas-v10.txt` or whichever
  numbered version is current).
- Do not commit files under `/tmp/` or `.bak` / `.bakN` - both are
  git-ignored.

## Repository layout

```
.
|-- .gitignore
|-- LICENSE
|-- README.md
|-- AGENTS.md                  # authoring-time agent instructions (not consumed at runtime)
|-- manifest.json              # the mod entry point the engine reads
|-- preview.png                # workshop thumbnail
|-- Art/
|   |-- Flags/
|   `-- Portraits/
|-- Data/                      # all game-data tables (JSON, Newtonsoft-friendly)
|-- Maps/
|   `-- DefaultVoronoiMap.json # 1750 Americas Voronoi geometry (~54 MB, locked)
|-- Prompts/                   # authoring-time SDXL / knowledge artifacts
`-- Skills/
    `-- historical-ink-portrait/  # the portrait style guide used to generate Art/
```
