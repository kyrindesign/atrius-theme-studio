# Atrius Theme Studio

A browser tool for designing the look of our Atrius (LocusLabs) indoor maps. A live map sits on the left and a panel of theme settings on the right. Each change is previewed on the real map, and the result can be exported for the SDK config or for the Atrius team.

**Open it:** https://kyrindesign.github.io/atrius-theme-studio/

## What you can change

| Section | What it controls | Where it ends up |
|---|---|---|
| **Documented keys** | UI colors from the [Atrius theming docs](https://docs.atrius.com/docs/theming-1): buttons, text, panels, widgets, flight-status chips | `theme.colors` in `LMInit.newMap(...)` |
| **Map place colors** | Shape fill for each place category (food, shops, restrooms…), the base map (background/tarmac, roads, buildings, floors) and badge colors | Atrius's per-venue map file (`theme.json`), plus `categoryBadge*` keys |
| **Extended · SDK internal** | Extra keys found in the SDK's built-in theme (category badges, markers, system statuses) | `theme.colors` (undocumented, so confirm with Atrius) |

- **☀ Light / ☾ Dark:** separate palettes for each mode.
- **Presets:** SDK defaults, the docs sample, and **Blueish Maps**, which is built from United's brand palette: a bluish map in both modes, with UI, badge and map colors for each. A preset is a complete look (light and dark palettes plus map colors) and applies immediately.
- **Generate from one color:** pick a brand color, and optionally *Soft* or *Bold* map tone. It builds a complete look for light and dark. The base map and UI use your color. Places and badges keep their own hues, blended toward it so they stay distinct but match. Text and buttons are held to WCAG AA contrast.
- **Apply:** edits wait for **Apply changes** unless *Apply changes automatically* is on.
- **Export:** *UI config* is a paste-ready `LMInit.newMap` config. *Map colors* is JSON for the Atrius team, with either just your changes or the whole edited venue file.
- **Import:** paste a UI config or a map-colors JSON back in.

## Sharing designs

Your edits are saved **in your own browser only** (localStorage). Nothing is shared automatically. To pass a design to a teammate, use **Export → Copy** and have them **Import** it.

## Caveats

- **The map colors are a preview.** They're applied in your browser through an undocumented SDK option (`dataFetch`). Real changes need Atrius to update the venue's map file, so send them the *Map colors* export.
- **Dark map and badge colors** use undocumented SDK internals. The dark map comes from a hidden debug toggle, and badges are recolored in the map's images and icons. Atrius only documents dark mode for iOS and Android.
- **Flight status** needs the Atrius Flight Status add-on. The demo account shows a "service error".
- **Airports:** pick one under *Airport & account*. The studio starts on Atrius's public demo account (`A11F4Y6SZRXH4X`), which has LA (LAX), Seattle (SEA), London Stansted (STN) and São Paulo (GRU). For other airports such as ATL or ORD, paste an Atrius account ID that includes them and press **Load**. The dropdown then lists that account's airports.

## Run locally

```bash
python3 -m http.server 5173
```

Then open http://localhost:5173. It's a single `index.html` with no build step. It needs internet access to load the Atrius SDK and map data.
