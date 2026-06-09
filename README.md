# Windows 11 — Home Assistant Theme

A Home Assistant theme inspired by Microsoft's **Fluent Design** language used in Windows 11. **No add-ons required** — works on stock Home Assistant 2026.x with native `backdrop-filter` blur on cards.

- Mica / Acrylic translucent surfaces with **real** backdrop blur on cards (via HA's built-in `--ha-card-backdrop-filter`)
- Segoe UI Variable typography (with safe fallbacks on non-Windows clients)
- 8 px card radius, 4 px control radius (Win11 corner system)
- Windows 11 accent color (`#0078D4` light / `#4CC2FF` dark)
- Single theme with `modes:` so HA's **Auto** option follows your OS theme
- Uses both the new HA 2026.x `--ha-color-*` token system and all legacy `--paper-*` / `--mdc-*` / `--primary-*` aliases — works across HA versions

| Light Mica | Dark Mica |
| :--------: | :-------: |
| `#F3F3F3` background, `#0078D4` accent | `#202020` background, `#4CC2FF` accent |

---

## Installation

### Option 1 — HACS (recommended)

1. HACS → ⋮ → **Custom repositories** → add this repo's URL with category **Theme**.
2. Find **Windows 11** in HACS → Themes, install, then restart Home Assistant.
3. Open your **Profile** (bottom-left avatar) and pick **Windows 11** as the theme. Set **Theme mode** to **Auto** to follow the OS.

### Option 2 — Manual

1. Copy [themes/windows11.yaml](themes/windows11.yaml) into your Home Assistant `config/themes/` folder.
   - Create the `themes/` folder if it does not exist.
2. Add the following to `configuration.yaml` (skip if you already have a `frontend:` block with `themes:`):

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

3. Restart Home Assistant (**Developer Tools → YAML → Restart**, or full restart).
4. Select **Windows 11** in your Profile as above.

> **Tip:** The Mica blur effect on cards looks best when the dashboard background isn't pure flat color. Set a wallpaper via a `picture` view background or the [browser_mod](https://github.com/thomasloven/hass-browser_mod) integration for the most dramatic frosted-glass look.

---

## Known limitations

### Menu and dialog blur

Cards get real `backdrop-filter` blur because HA exposes `--ha-card-backdrop-filter` as a theme variable. **Dropdown menus** (the ⋮ row-action menus) and **dialogs** (more-info, settings popups) do not have equivalent variables in HA 2026.x — and Web Awesome's `wa-popup` positioning (static layout + floating-ui transforms) breaks `backdrop-filter` even when applied directly via injected CSS.

These surfaces still receive a Mica-style translucent fill via `--ha-dialog-surface-background` and `--wa-color-surface-raised`, so they look reasonably Win11-flavored — they just don't blur the content behind them. This requires changes in the HA frontend or Web Awesome upstream; a theme alone cannot fix it.

---

## Backgrounds (wallpaper)

The Mica blur effect on cards only "pops" when there's something interesting behind them. The theme ships **8 Win11-style wallpapers** in the [backgrounds/](backgrounds/) folder, and a sensible default is enabled out of the box:

- **Default (both light & dark modes):** `RadialGradientBlue.jpg` (matches the official Windows 11 default wallpaper feel)

Wallpapers are served via the [jsDelivr CDN](https://www.jsdelivr.com/) directly from this repo — **no manual download needed** when installing via HACS. They apply to every dashboard view automatically via the native HA theme variable `lovelace-background`.

### Switching to a different bundled wallpaper

Edit [themes/windows11.yaml](themes/windows11.yaml) and change the filename in the `lovelace-background` line under the `light:` or `dark:` block. Bundled options (preview each by clicking):

| Filename | Mood |
| --- | --- |
| [`Chain.jpg`](backgrounds/Chain.jpg) | Industrial, monochrome |
| [`DarkGreyLittleSquares.jpg`](backgrounds/DarkGreyLittleSquares.jpg) | Subtle dark grid |
| [`Leaf.jpg`](backgrounds/Leaf.jpg) | Organic green |
| [`MinimalistMountains.jpg`](backgrounds/MinimalistMountains.jpg) | Soft pastel landscape |
| [`MountainStream.jpg`](backgrounds/MountainStream.jpg) | Cool blues / nature |
| [`RadialGradientBlue.jpg`](backgrounds/RadialGradientBlue.jpg) | Win11 hero gradient (default) |
| [`Railroad.jpg`](backgrounds/Railroad.jpg) | Linear repeating pattern |
| [`SunshineThroughMountains.jpg`](backgrounds/SunshineThroughMountains.jpg) | Warm golden hour |

For example to use `MountainStream.jpg` in dark mode:

```yaml
dark:
  lovelace-background: 'center / cover no-repeat fixed url("https://cdn.jsdelivr.net/gh/tedr91/ha-windows11-theme@main/backgrounds/MountainStream.jpg")'
```

### Using local copies (faster, works offline)

If you'd rather host the images locally:

1. Download the desired image(s) from the [backgrounds/](backgrounds/) folder
2. Place them in your Home Assistant `config/www/backgrounds/` folder (create it if it doesn't exist)
3. Replace the URL in the theme with the `/local/...` path:

   ```yaml
   light:
     lovelace-background: 'center / cover no-repeat fixed url("/local/backgrounds/RadialGradientBlue.jpg")'
   ```

4. Reload themes (Developer Tools → YAML → **Reload Themes**)

### Disabling the wallpaper

Set `lovelace-background` to a solid color or remove the line entirely:

```yaml
light:
  lovelace-background: 'var(--primary-background-color)'   # flat color
```

### Per-view overrides

Any view can override the theme-level wallpaper using the standard HA view background config — set it in the view editor under **View settings → Background**, or in YAML:

```yaml
views:
  - title: Bedroom
    background:
      image: /local/backgrounds/MountainStream.jpg
      size: cover
      alignment: center
      attachment: fixed
```

---

## Customizing the accent color

Windows 11 lets users pick a system accent. To match yours, edit [themes/windows11.yaml](themes/windows11.yaml) and change these in **both** the `light:` and `dark:` mode blocks:

```yaml
ha-color-primary-40: '#0078D4'     # ← your accent (hex) — light mode default
ha-color-primary-60: '#4CC2FF'     # ← your accent (lighter) — dark mode default
primary-color: '#0078D4'           # ← legacy alias, same as ha-color-primary-40/60 per mode
accent-color:  '#0078D4'           # ← same as primary-color
rgb-primary-color: '0, 120, 212'   # ← same color in R, G, B
rgb-accent-color: '0, 120, 212'
```

Common Windows 11 accents:

| Name           | Hex (Light) | Hex (Dark) |
| -------------- | ----------- | ---------- |
| Default Blue   | `#0078D4`   | `#4CC2FF`  |
| Yellow Gold    | `#FFB900`   | `#FFC83D`  |
| Orange         | `#CA5010`   | `#F7630C`  |
| Red            | `#E81123`   | `#FF99A4`  |
| Pink           | `#E3008C`   | `#FF8FB9`  |
| Purple         | `#5C2E91`   | `#B4A0FF`  |
| Green          | `#107C10`   | `#6CCB5F`  |
| Teal           | `#038387`   | `#3AA0A4`  |

---

## File structure

```
ha-windows11-theme/
├── themes/
│   └── windows11.yaml      ← the theme
├── backgrounds/            ← bundled Win11-style wallpapers (served via jsDelivr CDN)
├── README.md
├── LICENSE
└── .gitignore
```

---

## Credits

- Color tokens & opacities derived from Microsoft's public **Fluent 2 / WinUI 3** design specifications.
- Built for Home Assistant 2026.x (also compatible with earlier versions via legacy token aliases).

## License

[MIT](LICENSE)
