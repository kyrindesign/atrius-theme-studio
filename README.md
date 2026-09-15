# Atrius Theme Studio

A browser tool for designing the look of our Atrius (LocusLabs) indoor maps. A live map sits on the left and a panel of theme settings on the right. Each change is previewed on the real map, and the result can be exported for the SDK config or for the Atrius team.

**Open it:** https://kyrindesign.github.io/atrius-theme-studio/

## What you can change

| Section | What it controls | Where it ends up |
|---|---|---|
| **Documented keys** | UI colors from the [Atrius theming docs](https://docs.atrius.com/docs/theming-1): buttons, text, panels, widgets, flight-status chips | `theme.colors` in `LMInit.newMap(...)` |
| **Map place colors** | Shape fill for each place category (food, shops, restrooms…), the base map (background/tarmac, roads, buildings, floors) and badge colors, which also color the location dots | Atrius's per-venue map file (`theme.json`), plus `categoryBadge*` keys |
| **Extended · SDK internal** | Extra keys found in the SDK's built-in theme (category badges, markers, system statuses) | `theme.colors` (undocumented, so confirm with Atrius) |

- **☀ Light / ☾ Dark:** separate palettes for each mode.
- **Presets:** SDK defaults, **FlightAware Blue** (a navy map from the FlightAware map reference) and **United Blue**, which uses only United color library colors everywhere: map, UI, badges, labels and every status (from United Alerts). Any leftover color is mapped to its closest library color when the preset is applied, and hard-coded map-style colors (POI-dot labels) are previewed and exported as `_mapStyleOverrides` for Atrius. Each sets UI, badge, map and label colors for light and dark, and leaves status, error and other system colors standard. A preset is a complete look and applies immediately. The dropdown shows the preset in use, and switches to *Custom* once you edit a color, generate or import.
- **Generate from one color:** pick a brand color, and optionally *Soft* or *Bold* map tone. It builds a complete look for light and dark. The base map and UI use your color. Places and badges keep their own hues, blended toward it so they stay distinct but match. Text and buttons are held to WCAG AA contrast.
- **Apply:** edits wait for **Apply changes** unless *Apply changes automatically* is on.
- **Export:** *UI config* is a paste-ready `LMInit.newMap` config. *Map colors* is JSON for the Atrius team, with either just your changes or the whole edited venue file.
- **Import:** paste a UI config or a map-colors JSON back in.
- **Fix SDK text contrast** (on by default): some SDK text ignores the theme and is hard-coded `#333333`/`#666666`/`#000000`: place-card descriptions, website and phone links, map menu items, and flight-status error text. On dark panels that fails contrast. The studio previews a small host-page CSS rule that points these elements at the theme's own colors. Copy it from **Export → Host CSS** into the page that embeds the map; the map renders in that page, so the rule applies in production.

## Sharing designs

Your edits are saved **in your own browser only** (localStorage). Nothing is shared automatically. To pass a design to a teammate, use **Export → Copy** and have them **Import** it.

## Caveats

- **The map colors are a preview.** They're applied in your browser through an undocumented SDK option (`dataFetch`). Real changes need Atrius to update the venue's map file, so send them the *Map colors* export.
- **Dark mode place cards:** the SDK hard-codes the description, website and phone text as `#333333`, and no theme key changes it. On a dark panel those three lines fail contrast (about 1.4:1). Every other card text passes. Raise it with Atrius before shipping dark mode.
- **Dark map and badge colors** use undocumented SDK internals. The dark map comes from a hidden debug toggle, and badges are recolored in the map's images and icons. Atrius only documents dark mode for iOS and Android.
- **Flight status** needs the Atrius Flight Status add-on. The demo account shows a "service error".
- **Airports:** pick one under *Airport & account*. The studio starts on Atrius's public demo account (`A11F4Y6SZRXH4X`), which has LA (LAX), Seattle (SEA), London Stansted (STN) and São Paulo (GRU). For other airports such as ATL or ORD, paste an Atrius account ID that includes them and press **Load**. The dropdown then lists that account's airports.

## Run locally

```bash
python3 -m http.server 5173
```

Then open http://localhost:5173. It's a single `index.html` with no build step. It needs internet access to load the Atrius SDK and map data.
