# Setting Up icons.yaml

The `icons.yaml` configuration defines product context and style DNA. The file is created at the project root once the styling direction is established. Real icon creation begins only after `icons.yaml` exists.

---

## 1. Project Setup Flow

1. Inspect project environment:
   - Identify frontend framework, bundler, and existing SVG loader (Vite + React, Next.js, Vue, Svelte).
   - Identify current icon library (Lucide, Heroicons, Tabler, etc.) and displayed pixel sizes (e.g. 16px, 20px, 24px).
   - Inspect typography and border radii on existing buttons and cards.
   - Determine component directory structure (e.g. `src/components/icons` or `src/icons`).
2. Synthesize findings and present concrete options:
   - Summarize project findings in 1 to 2 lines.
   - Present specific multiple-choice options for product summary, personality, folder path, and style.
3. Generate candidate looks:
   - Generate 2 to 3 candidate DNAs using presets (`plain`, `soft`, `crisp`, `pixel`).
   - Draw 4 to 6 test icons in temporary candidate folders.
   - Run `node scripts/icons.js preview <template.html> --candidates <tmp>/a.yaml,<tmp>/b.yaml`.
   - Present the candidates to the user.
4. Finalize configuration:
   - Write the chosen configuration to `icons.yaml` at the project root.
   - Transfer winning test source files to `<dir>/source/`.
   - Run `node scripts/icons.js check` to confirm `complete`.

---

## 2. Configuration Schema

```yaml
product: Invoicing web app for design studios
personality: [calm, friendly, precise]
sizes: [16, 20, 24]
dir: src/components/icons
avoid: [letters inside icons, dollar signs]

dna:
  style: outline
  stroke: 2
  grid: 24
  padding: 1
  keylines: { circle: 20, square: 18, portrait: [16, 20], landscape: [20, 16] }
  corners: { family: circular, radius: { large: 2, small: 1 }, largeThreshold: 8 }
  caps: round
  joins: round
  angles: [0, 45, 90]
  detail: low
  minGap: 2
  badge: { position: bottom-right, size: 8, gap: 2 }
  duotoneOpacity: 0.2
```

### Field Definitions

| Field | Description | Source |
|---|---|---|
| `product` | One-line description of product purpose and target users | Project inspection or user clarification |
| `personality` | Three descriptive terms guiding metaphor and corner choice | User selection |
| `sizes` | Integer list of pixel sizes rendered in the UI (e.g. `[16, 20]`) | Codebase usage scan |
| `dir` | Target path where `source/` and compiled SVGs live | Project folder structure |
| `avoid` | Explicit list of disallowed metaphors or symbols | Project constraints |
| `dna.style` | `outline`, `duotone`, or `filled` | Design choice |
| `dna.stroke` | Stroke width in grid units (standard: 2) | Preset or candidate preview |
| `dna.grid` | Coordinate canvas dimension (standard: 24) | Preset |
| `dna.padding` | Empty outer margin in grid units (standard: 1) | Preset |
| `dna.keylines` | Standard geometric reference dimensions | Preset |
| `dna.corners` | Corner family (`circular`, `squircle`, `sharp`) and radii | UI radius match |
| `dna.caps` | Line end style (`round`, `butt`, `square`) | Personality match |
| `dna.joins` | Corner join style (`round`, `miter`, `bevel`) | Corner family match |
| `dna.angles` | Allowed line angles in degrees (standard: `[0, 45, 90]`) | Preset |
| `dna.detail` | Stroke budget: `minimal`, `low`, `medium`, or `high` | Display sizes |
| `dna.minGap` | Minimum visible clearance between ink boundaries | Stroke clearance math |
| `dna.badge` | Modifier badge corner placement, size, and gap | Preset |
| `dna.pixel` | Set `true` for strict retro pixel-art sets | Retro project requirements |

---

## 3. Matching DNA to Product Design Language

- Stroke weight matches body text: UI with 14px regular body text pairs with 1.5 stroke. Heavier, bold consumer UI pairs with 2.0 or 2.5 stroke.
- Corners echo UI elements: Pill buttons and curved cards align with squircle or large circular radii (radius 2 or 3). Sharp-cornered, technical tools align with sharp corners and miter joins.
- Small display sizes reduce detail: If icons appear predominantly at 16px, lock `dna.detail` to `minimal` or `low` and ensure `dna.minGap >= 2`.

---

## 4. Candidate Previews

Test candidate styles in the product's actual UI before drawing full sets:
1. Select 4 to 6 contrasting concepts (e.g. house, search, settings, user, document, plus).
2. Create temporary candidate directories:
   - `/tmp/candidate-plain.yaml` and `/tmp/candidate-plain/`
   - `/tmp/candidate-soft.yaml` and `/tmp/candidate-soft/`
3. Generate sample HTML template mimicking the application navigation bar or sidebar with `<i data-icon="house"></i>` placeholders.
4. Run:
   ```bash
   node scripts/icons.js preview /tmp/nav-template.html --candidates /tmp/candidate-plain.yaml,/tmp/candidate-soft.yaml
   ```
5. Confirm visual choice with the user, copy winning settings to `icons.yaml`, and move the source SVGs into `<dir>/source/`.

---

## 5. Maintenance and DNA Updates

- Adjusting stroke weight: Update `dna.stroke`, execute `node scripts/icons.js lint all`, fix flagged clearance errors, and run `build`.
- Modifying other DNA properties: Changing corner family, grid, or style requires redrawing affected geometry. Confirm with the user prior to updating.
- Adding supported sizes: When UI introduces a new size (e.g. 20px), append the integer to `sizes` in `icons.yaml`.
