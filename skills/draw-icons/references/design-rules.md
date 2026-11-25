# Icon Design and Geometry Rules

These rules define the geometry standards for drawing consistent SVG icon sets. The base measurements below use the standard DNA (24 grid, padding 1, stroke 2). Scale proportionally if your project's `icons.yaml` specifies a different grid or stroke.

## Contents
1. Think in Centrelines
2. Canvas, Keylines, and Optical Weight
3. Corner Families and Tangent Math
4. Stroke Geometry, Terminations, and Dots
5. Gap Calculations and Clearance
6. Curves and Handle Alignment
7. Angles and Grid Snapping
8. Detail Budgets and Metaphor Selection
9. Modifier Badges and Slashes
10. Large Circular Overlays

---

## 1. Think in Centrelines

Every coordinate placed in a source SVG represents the center of a stroke. Ink expands `stroke / 2` in both directions from the centreline.

Example with stroke 2:
- A horizontal line at `y = 3` produces ink spanning `y = 2` to `y = 4`.
- A vertical line at `x = 20` produces ink spanning `x = 19` to `x = 21`.

Build all icons from pure geometric primitives:
- Straight lines
- Circular arcs (command `A`)
- Circles (`circle`)
- Rectangles (`rect`)

Avoid freehand cubic beziers. Reserve bezier curves strictly for natural shapes that cannot be represented by circular arcs.

---

## 2. Canvas, Keylines, and Optical Weight

### Canvas Boundary
- Grid viewBox: `0 0 24 24`.
- Canvas padding: `padding = 1`.
- Outer ink limit: centrelines must sit between `2` and `22` so ink does not clip at the border.

### Standard Keylines
Shapes with identical bounding boxes do not have equal visual mass. Circles look smaller than squares of identical width. Use these keyline dimensions to achieve visual parity:
- Circle keyline: 20 units across (`cx=12 cy=12 r=10`).
- Square keyline: 18x18 units (from `x=3, y=3` to `x=21, y=21`).
- Portrait rectangle: 16x20 units (from `x=4, y=2` to `x=20, y=22`).
- Landscape rectangle: 20x16 units (from `x=2, y=4` to `x=22, y=20`).

### Optical Adjustments
- Pointy shapes overshoot: Triangles, diagonal arrows, and sharp crests appear optically small. Push tips 0.5 to 1 unit beyond standard keyline bounds.
- Visual center vs mathematical center: Symmetrical icons center at (12, 12). Asymmetric shapes (play arrows, speech bubble tails) must offset 0.5 to 1 unit toward the lighter side so optical mass remains centered.
- Minimum coverage: Completed icons must span 16 to 20 units across. Any shape under 12 units appears lost next to siblings.

---

## 3. Corner Families and Tangent Math

Corner style is locked by `dna.corners` in `icons.yaml`. Never mix corner families within a project.

### Corner Styles
1. circular: Real circular arcs. Use `radius.large` (2) on shapes with width and height >= `largeThreshold` (8). Use `radius.small` (1) on smaller elements. When two diagonal strokes meet at a 90-degree corner, multiply radius by 1.2071 (approx 2.41) so the corner matches horizontal/vertical corners optically.
2. squircle: Smooth continuous-curvature corner (iOS style). Never write `<rect rx>`. Always generate path data via:
   `node scripts/icons.js shape rect X Y W H [R]` or `node scripts/icons.js shape poly "..." --closed`.
3. sharp: Zero rounding (`radius = 0`). Joins use `miter`. Keep acute angles above 40 degrees to prevent spikes.

### Rules
- Round stroke joins (`stroke-linejoin="round"`) do not replace corner rounding. 90-degree corners in circular or squircle sets require explicit geometric radius in the path data.
- Do not round junctions where 3 or more lines intersect.
- Concentric outer and inner corners must share the same radius setting.

---

## 4. Stroke Geometry, Terminations, and Dots

- Uniform width: All icons in a project share `dna.stroke`. Never write `stroke-width`, `stroke`, or `fill` inside source files.
- Stroke terminations: Open ends receive `dna.caps` during compilation. An open stroke approaching another element must either terminate cleanly at least `stroke + minGap` away or meet it directly on its centreline. Never leave half-gaps.
- Dots: A dot is a zero-length horizontal move with round caps:
  `M12 17h.01`
  During build with round caps, this creates a clean circle matching the stroke diameter.
- Continuous paths: Prefer single continuous paths over segmented fragments. Overlapping strokes darken when rendered with opacity.

---

## 5. Gap Calculations and Clearance

Separate elements must maintain a minimum centreline distance:
`clearance >= stroke + minGap`

For standard DNA (stroke 2, minGap 2):
- Minimum centreline separation: `2 + 2 = 4` units.
- Parallel lines in documents: place at `y=13` and `y=17` (distance 4), never closer.
- Detail strokes inside containers: maintain at least 4 units from container centrelines.
- Inner holes: minimum 4 units clearance across the opening.

The `lint` command measures all clearances. If flagged, space elements apart or simplify details.

---

## 6. Curves and Handle Alignment

- Circles and circular arcs: Use `<circle>` or SVG arc command `A`. Never approximate circles with bezier curves.
- Handle alignment: Where two curve segments join, their tangent vectors must be collinear. A mismatch between 1.5 degrees and 25 degrees triggers a `kink` lint warning.
- Node economy: Use the absolute minimum number of nodes required. Extra points cause visible wobbles at small scale.

---

## 7. Angles and Grid Snapping

- Allowed line orientations: Keep straight lines constrained to `dna.angles` (typically 0, 45, and 90 degrees).
- Coordinate snapping:
  - Even stroke (stroke 2): place endpoints and arc centers on whole integers.
  - Odd stroke (stroke 1, 3): place endpoints on half-integers (`n.5`).
  - Stroke 1.5: place endpoints on quarter-integers (`n.25` or `n.75`).
- Precision: Round all path coordinates to a maximum of 3 decimal digits.
- Pixel sets (`dna.pixel: true`):
  - Every grid unit equals 1 screen pixel.
  - Horizontal and vertical runs only.
  - Curves and diagonals are represented as stepped stairs.
  - No circles, ellipses, or rounded rects allowed.

---

## 8. Detail Budgets and Metaphor Selection

### Metaphors
- Depict the object, not the action: "Save" is a disk or tray; "Settings" is a gear; "Notifications" is a bell.
- Stick to established universal conventions. Avoid creating novel metaphors for established patterns.
- Keep drawings strictly two-dimensional (flat, front or side view). No perspective, shadows, or gradients.
- Do not place letters or numbers inside icons unless the symbol itself is the character.

### Detail Budgets (dna.detail)
- minimal: approx 4 stroke segments.
- low: approx 6 stroke segments (standard default).
- medium: approx 9 stroke segments.
- high: approx 13 stroke segments.

Test rule: verify if the icon remains immediately legible when scaled down to 16px. If it clogs, remove non-essential strokes.

---

## 9. Modifier Badges and Slashes

Modifier badges must maintain uniform size and placement across all icons:
- Badge placement: Bottom-right corner (slot size: 8x8 units, x=15 to 23, y=15 to 23 with padding 1).
- Base shape clearance: Trim base paths so their ink terminates at least `stroke + minGap` away from the badge area. Never let the base shape collide with badge strokes.
- Slashes (disabled/off state): Diagonal slash `M2 2l20 20`. Cut the base shape with clearance on both sides of the slash.

---

## 10. Large Circular Overlays

When placing larger overlays (clock, lock, status indicator) over a base shape:
- Overlay size: circle with radius approx 6 units positioned at a corner (e.g. center at (16, 16)).
- Base cutback: Cut the base shape along a circle of `radius = r_overlay + stroke + minGap` centered at the overlay center.
- Internal simplicity: Keep overlay interior minimal (e.g. two clock hands or simple keyhole).
- Silhouettes: When generating duotone or filled icons, terminate the base silhouette along the cutout boundary so a clean moat separates the two shapes.
