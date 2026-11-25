# Icon Recipes and Canonical Geometry

This reference provides exact coordinates and verified geometry for common app icons on the standard 24x24 grid with stroke 2, padding 1, and circular corners (radius 2 for large shapes, radius 1 for small shapes). Adapt radii with the `shape` CLI when working in squircle or sharp sets.

## Rules for Using Recipes

1. All coordinates represent stroke centrelines.
2. Elements must contain geometry attributes only: `d`, `cx`, `cy`, `r`, `x`, `y`, `width`, `height`, `x1`, `y1`, `x2`, `y2`, `points`.
3. Never include `stroke`, `fill`, `class`, or `style` attributes in source files.
4. Outline icons use no `data-role`.
5. Duotone icons mark the main body shape with `data-role="silhouette"`.
6. Filled icons mark the main body shape with `data-role="silhouette"` and all internal cutouts with `data-role="detail"`.

---

## Shared Parts (store in source/parts/)

### 1. Document Container (source/parts/document.svg)
- Body: `M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z`
- Fold: `M14 2v4a2 2 0 0 0 2 2h4`
- In filled sets: body gets `data-role="silhouette"`, fold gets `data-role="detail"`.

### 2. Person Bust (source/parts/person.svg)
- Head: `<circle cx="12" cy="7" r="4"/>`
- Shoulders: `<path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/>`
- In filled sets: head gets `data-role="silhouette"`, shoulders path closed with `H19Z` gets `data-role="silhouette"`.

### 3. Magnifier Glass (source/parts/lens.svg)
- Lens: `<circle cx="11" cy="11" r="8"/>`
- Handle: `<path d="m21 21-4.34-4.34"/>`

### 4. Modifier Badges (size 8x8 at bottom-right, ink in x=15 to 23, y=15 to 23)
- Plus badge: `<path d="M19 16v6M16 19h6"/>`
- Check badge: `<path d="m16 19 2 2 4-4"/>`
- Cross badge: `<path d="m16 16 6 6M22 16l-6 6"/>`

---

## Core Icon Recipes (24x24 Canvas)

### House (Home)
- Metaphor: Roof peak with rectangular base and optional door.
- Elements:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="home dashboard">
  <path d="M2 12L10.586 3.414A2 2 0 0 1 13.414 3.414L22 12"/>
  <path d="M4 10v10a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V10"/>
  <path d="M9 22v-6a2 2 0 0 1 2-2h2a2 2 0 0 1 2 2v6"/>
</svg>
```

### Search
- Metaphor: Lens pointing down-right.
- Elements:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="find explore magnifier">
  <circle cx="11" cy="11" r="8"/>
  <path d="m21 21-4.5-4.5"/>
</svg>
```

### Gear (Settings)
- Metaphor: Central axle hole with 8 outer cogs.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="settings preferences options" data-waive-heavy="8-tooth gear requires extended perimeter">
  <circle cx="12" cy="12" r="3"/>
  <path d="M20.157 10.507L20.788 10.559A1 1 0 0 1 21.705 11.556L21.705 12.444A1 1 0 0 1 20.788 13.441L19.525 13.546A1 1 0 0 0 18.685 14.16L18.254 15.199A1 1 0 0 0 18.415 16.228L19.233 17.195A1 1 0 0 1 19.176 18.548L18.548 19.176A1 1 0 0 1 17.195 19.233L16.228 18.415A1 1 0 0 0 15.199 18.254L14.16 18.685A1 1 0 0 0 13.546 19.525L13.441 20.788A1 1 0 0 1 12.444 21.705L11.556 21.705A1 1 0 0 1 10.559 20.788L10.454 19.525A1 1 0 0 0 9.84 18.685L8.801 18.254A1 1 0 0 0 7.772 18.415L6.805 19.233A1 1 0 0 1 5.452 19.176L4.824 18.548A1 1 0 0 1 4.767 17.195L5.585 16.228A1 1 0 0 0 5.746 15.199L5.315 14.16A1 1 0 0 0 4.475 13.546L3.212 13.441A1 1 0 0 1 2.295 12.444L2.295 11.556A1 1 0 0 1 3.212 10.559L4.475 10.454A1 1 0 0 0 5.315 9.84L5.746 8.801A1 1 0 0 0 5.585 7.772L4.767 6.805A1 1 0 0 1 4.824 5.452L5.452 4.824A1 1 0 0 1 6.805 4.767L7.772 5.585A1 1 0 0 0 8.801 5.746L9.84 5.315A1 1 0 0 0 10.454 4.475L10.559 3.212A1 1 0 0 1 11.556 2.295L12.444 2.295A1 1 0 0 1 13.441 3.212L13.546 4.475A1 1 0 0 0 14.16 5.315L15.199 5.746A1 1 0 0 0 16.228 5.585L17.195 4.767A1 1 0 0 1 18.548 4.824L19.176 5.452A1 1 0 0 1 19.233 6.805L18.415 7.772A1 1 0 0 0 18.254 8.801L18.685 9.84A1 1 0 0 0 19.525 10.454Z"/>
</svg>
```

### Bell (Notifications)
- Metaphor: Symmetrical bell chime with clapper dot at bottom.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="notifications alert">
  <path d="M10.268 21a2 2 0 0 0 3.464 0"/>
  <path d="M6 8a6 6 0 0 1 12 0c0 7 3 9 3 9H3s3-2 3-9Z"/>
</svg>
```

### User (Profile)
- Metaphor: Circular head over rounded shoulder curve.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="profile account person">
  <circle cx="12" cy="7" r="4"/>
  <path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/>
</svg>
```

### Document (File Text)
- Metaphor: Sheet with folded top-right corner and horizontal text lines.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="invoice file document">
  <path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z"/>
  <path d="M14 2v4a2 2 0 0 0 2 2h4"/>
  <path d="M8 13h8"/>
  <path d="M8 17h5"/>
</svg>
```

### Trash (Delete)
- Metaphor: Open bin can with lid and top handle.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="delete remove bin">
  <path d="M3 6h18"/>
  <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/>
  <path d="M9 6V4a2 2 0 0 1 2-2h2a2 2 0 0 1 2 2v2"/>
  <line x1="10" y1="11" x2="10" y2="17"/>
  <line x1="14" y1="11" x2="14" y2="17"/>
</svg>
```

### Pencil (Edit)
- Metaphor: Angled pen shaft with tip pointing down-left.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="edit write compose">
  <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5Z"/>
  <path d="m15 5 4 4"/>
</svg>
```

### Check (Done / Success)
- Metaphor: Distinct checkmark, short leg down to right, long leg up to right.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="confirm done success">
  <path d="M20 6 9 17l-5-5"/>
</svg>
```

### Close (Cancel / Remove)
- Metaphor: Symmetrical cross strokes.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="cancel dismiss cross">
  <path d="m18 6-12 12M6 6l12 12"/>
</svg>
```

### Plus (Add / Create)
- Metaphor: Horizontal and vertical cross strokes centered at (12, 12).
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="add new create">
  <path d="M5 12h14M12 5v14"/>
</svg>
```

### Minus (Collapse / Subtract)
- Metaphor: Single horizontal line centered at (12, 12).
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="subtract collapse remove">
  <path d="M5 12h14"/>
</svg>
```

### Chevron Right
- Metaphor: 90-degree corner chevron pointing right.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="next forward chevron">
  <path d="m9 18 6-6-6-6"/>
</svg>
```

### Arrow Right
- Metaphor: Stem line through center with arrow head at tip.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="arrow next proceed">
  <path d="M5 12h14"/>
  <path d="m12 5 7 7-7 7"/>
</svg>
```

### Lock
- Metaphor: Rectangular body with arched shackle on top.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="security private password">
  <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
  <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
</svg>
```

### Eye
- Metaphor: Two matching arcs meeting at corners with centered pupil circle.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="view show inspect">
  <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7Z"/>
  <circle cx="12" cy="12" r="3"/>
</svg>
```

### Mail (Envelope)
- Metaphor: Landscape rectangle with V-fold from top corners.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="email message inbox">
  <rect x="2" y="4" width="20" height="16" rx="2"/>
  <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
</svg>
```

### Folder
- Metaphor: Tabbed folder container.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="directory folder files">
  <path d="M4 20h16a2 2 0 0 0 2-2V8a2 2 0 0 0-2-2h-7.93a2 2 0 0 1-1.66-.9l-.82-1.2A2 2 0 0 0 7.93 3H4a2 2 0 0 0-2 2v13c0 1.1.9 2 2 2Z"/>
</svg>
```

### Shield
- Metaphor: Symmetrical coat of arms tapering to bottom point.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="security protect shield">
  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10Z"/>
</svg>
```

### Clock
- Metaphor: Circle container with center-pinned hands at 90 degrees.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="time clock history">
  <circle cx="12" cy="12" r="10"/>
  <polyline points="12 6 12 12 16 16"/>
</svg>
```

### Calendar
- Metaphor: Rectangular pad with top hanging pins and horizontal header separator.
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-aliases="date calendar schedule">
  <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
  <line x1="16" y1="2" x2="16" y2="6"/>
  <line x1="8" y1="2" x2="8" y2="6"/>
  <line x1="3" y1="10" x2="21" y2="10"/>
</svg>
```
