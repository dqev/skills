# Review Checklist and Lint Rules

Linting verifies mathematical constraints. Visual inspection verifies optical balance, recognition at small scale, and harmony with adjacent icons. Rendered PNG files are written to a temporary review directory printed by the CLI.

---

## 1. Visual Inspection Routine

Execute `node scripts/icons.js render <names>` and inspect the generated PNG block per icon:

1. Legibility at minimum size:
   - Check the 16px tile. The symbol must be identifiable without reading its label.
   - If internal details blur into a solid block, remove secondary strokes or provide a `<name>@16.svg` variant.
2. Keyline compliance:
   - The primary silhouette must reach its corresponding keyline boundary (circle, square, or rectangle).
   - Ensure the shape does not float undersized within the canvas or protrude into padding.
3. Optical centering:
   - Verify that asymmetrical masses (such as play chevrons or speech bubbles) feel centered by eye, not just by mathematical bounding box.
4. Clean node distribution:
   - Review the construction view. Red nodes must sit on grid intersections or clean tangent boundaries.
   - Eliminate redundant nodes lying along straight segments.
5. Clearance verification:
   - In pixel tiles, confirm that all parallel strokes and inner cutouts remain visible and distinct.
   - Any clogged region requires moving lines further apart or deleting one element.
6. Silhouette readability (filled icons):
   - Internal details cut through the silhouette via masks. Ensure solid outer material completely encloses cutouts.
7. Grid crispness:
   - Horizontal and vertical strokes must align to clean pixel edges without blurry half-lit pixels.

---

## 2. Set Sheet Inspection

Execute `node scripts/icons.js sheet <names> <neighbors>` to evaluate a batch alongside sibling icons from the same UI section:

1. Blur test:
   - Inspect the bottom blurred row. Each icon blob must show comparable optical density and diameter to its neighbors.
   - An excessively dark blob indicates surplus internal strokes or lines placed too closely.
   - An oversized blob indicates paths extending beyond keylines.
2. Anomaly detection:
   - If one icon stands out as foreign, verify corner radius consistency, stroke width, and terminal cap alignment.
3. Component consistency:
   - Shared elements (document outlines, busts, modifier badges) must match sibling coordinates exactly.

---

## 3. Comprehensive Lint Rule Dictionary

| Rule Name | Level | Cause | Required Fix |
|---|---|---|---|
| `name` | error | File name is not kebab-case | Rename file to lowercase with hyphens (e.g. `file-text.svg`) |
| `viewbox` | error | Root `viewBox` is not `0 0 <grid> <grid>` | Set `viewBox="0 0 24 24"` on `<svg>` |
| `attribute` | error | Prohibited attribute on `<svg>` or shape | Remove `stroke`, `fill`, `class`, or styling attributes. Source files contain geometry only |
| `element` | error | Prohibited tag present (e.g. `<g>`, `<title>`, `<circle>` inside groups) | Remove `<g>` wrapper tags and non-shape elements |
| `role` | error | `data-role` is not `silhouette` or `detail` | Use only `data-role="silhouette"` or `data-role="detail"` |
| `path` | error | Malformed SVG path syntax | Correct path command syntax |
| `decimals` | warn | Coordinate uses more than 3 decimal digits | Round numbers to maximum 3 decimals (e.g. `12.586`) |
| `silhouette-open` | error | Silhouette element is an unclosed path | Terminate path with `Z` or use a closed element |
| `silhouette-missing` | warn | Duotone or filled icon lacks a silhouette element | Add `data-role="silhouette"` to the body outline |
| `clipped` | error | Stroke ink extends outside canvas boundary | Move path inward so ink boundary clears canvas edge |
| `padding` | warn | Ink penetrates the defined padding margin | Adjust path endpoints to maintain `padding = 1` |
| `reversal` | error | Path doubles back directly over its own stroke | Eliminate backwards backtrack or split path |
| `kink` | warn | Two curves meet at an accidental angle (1.5 to 25 deg) | Align tangent vectors collinear across the join |
| `gap` | error | Distance between separate strokes is under `stroke + minGap / 2` | Separate strokes to at least `stroke + minGap` or join them explicitly |
| `gap` | warn | Distance between separate strokes is under `stroke + minGap` | Space strokes apart or eliminate dense lines |
| `radius` | warn | Corner radius does not match project DNA | Use `radius.small` or `radius.large` defined in config |
| `angle` | warn | Line segment orientation violates `dna.angles` | Adjust line angle to 0, 45, or 90 degrees |
| `pixel` | error | Curve, diagonal, or off-grid point in a pixel-art set | Replace with orthogonal horizontal/vertical steps |
| `off-grid` | warn | Over 25% of endpoints fall off the crisp grid | Snap vertices to whole or half-unit grid coordinates |
| `detail-budget` | warn | Number of stroke segments exceeds `dna.detail` limit | Simplify geometry and remove non-essential strokes |
| `detail-outside` | warn | `data-role="detail"` element sits outside the silhouette | Remove `data-role="detail"` or move element inside silhouette |
| `hidden-in-filled` | warn | Stroke inside silhouette lacks `data-role="detail"` | Add `data-role="detail"` to cut out internal strokes |
| `heavy` | warn | Ink coverage exceeds 1.6x the keyline circle | Reduce internal density to prevent dark optical mass |
| `small` | warn | Geometry spans less than 55% of keyline dimension | Expand drawing to fill standard keyline footprint |
| `waiver` | error | Waiver rationale is under 12 characters or >3 waivers | Provide a concrete design explanation; fix geometry if >3 |

---

## 4. Lint Waivers

When geometry is intentional but triggers a lint warning, define an explicit waiver on the root `<svg>` tag:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-waive-angle="Roof pitch matches standard home architectural slope">
```

### Waiver Rules
- Only `warn` levels may be waived. `error` levels represent hard invalid states and cannot be waived.
- Rationale strings must contain 12 or more characters explaining the specific design intent.
- An individual icon may contain at most 3 waivers. Exceeding 3 indicates geometry requires structural refactoring.
