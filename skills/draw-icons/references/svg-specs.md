# Source SVG Specification and Compilation Model

All icon source files live in the directory specified by `dir` in `icons.yaml`:

```
<dir>/
  source/
    house.svg          Primary source (one per icon, kebab-case file name)
    house@16.svg       Optional simplified variant for small displays (<= 16px)
    parts/
      document.svg     Shared geometry copied into multiple icons
  house.svg            Compiled output written by build. Never edit manually.
```

---

## 1. Source File Format: Pure Geometry Only

Source files must be flat SVGs containing geometry coordinates only. Styling attributes are strictly prohibited in source files because `build` applies all stroke widths, colors, caps, and joins from `icons.yaml`.

### Allowed Elements
- `path` (attribute `d`)
- `line` (attributes `x1`, `y1`, `x2`, `y2`)
- `polyline` (attribute `points`)
- `polygon` (attribute `points`)
- `circle` (attributes `cx`, `cy`, `r`)
- `ellipse` (attributes `cx`, `cy`, `rx`, `ry`)
- `rect` (attributes `x`, `y`, `width`, `height`, optional `rx`, `ry`)

### Forbidden Elements and Attributes
- Prohibited tags: `<g>`, `<defs>`, `<style>`, `<use>`, `<title>`, `<desc>`, `<mask>`, `<clipPath>`.
- Prohibited attributes: `stroke`, `fill`, `stroke-width`, `stroke-linecap`, `stroke-linejoin`, `class`, `style`, `id`, `transform`.

---

## 2. Compilation Roles

In an outline set, every element in the source file represents a normal stroke.

In duotone and filled sets, specific elements are assigned roles via the `data-role` attribute:

| Attribute | Role | Duotone Compilation | Filled Compilation |
|---|---|---|---|
| (absent) | Normal stroke | Rendered with primary stroke | Rendered with stroke, merges into fill |
| `data-role="silhouette"` | Body outline | Rendered as solid fill at `duotoneOpacity` under strokes | Rendered filled and stroked |
| `data-role="detail"` | Internal feature | Rendered as normal stroke | Cut out of fill via SVG mask |

### Silhouette Rules
- Trace the outer boundary of the main icon body. Close the path with `Z` or use a closed element (`circle`, `rect`, `polygon`).
- Handles, badges, and protruding stems should remain normal strokes rather than part of the silhouette.
- Filled sets: any stroke that sits inside the silhouette must have `data-role="detail"`, otherwise it merges into the fill and disappears.
- Centerline clearance: internal detail strokes must maintain at least `stroke + minGap` distance from the silhouette boundary to prevent mask clipping.

---

## 3. Root SVG Attributes

The root `<svg>` tag must specify `viewBox="0 0 24 24"` (matching `dna.grid`). It may also define aliases and lint waivers:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="settings preferences" data-waive-heavy="6 teeth require extended path length">
```

### Supported Root Attributes
1. `data-style="filled"`: Defines an explicit solid/filled twin icon (e.g. `house-filled.svg` for active tab states) within an outline or duotone project.
2. `data-aliases="..."`: Space-separated list of product terms or synonym concepts. Enables discovery via alias searches.
3. `data-waive-<rule>="..."`: Explicit waiver for an intentional design exception. Must include a descriptive rationale of 12 or more characters. Maximum 3 waivers per icon.

---

## 4. Small Scale Variants (<name>@16.svg)

When `sizes` includes 16px or smaller and fine details clog at low resolution:
- Create `<name>@16.svg` alongside the primary source.
- Simplify internal geometry and reduce stroke count to meet the `minimal` detail budget (approx 4 strokes).
- The `build` command compiles this variant to `<dir>/<name>@16.svg` for direct import in low-resolution contexts.

---

## 5. Build Output

Running `build` executes full validation, clears existing built SVGs in `<dir>/`, and compiles each source into a clean, standalone SVG:
- Sets `viewBox="0 0 24 24"` and default `width="24" height="24"`.
- Sets `fill="none"` and `stroke="currentColor"`.
- Sets `stroke-width`, `stroke-linecap`, and `stroke-linejoin` from `dna`.
- Injects masks with deterministic IDs for filled cutouts.
- Built files use `currentColor` to dynamically inherit font and text colors in web frameworks.
