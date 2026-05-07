# Onefile — Feature Ideas

Sorted by development ease × impact. Top of each group = ship first.
Each entry: what it does, where the work lives, rough line count.

---

## Group 1 — Wire existing plumbing (under an hour each)

No new architecture. The state, functions, or DOM hooks already exist.

### 1. Unsaved-changes indicator
Show a `●` before the filename in `document.title` and `#filename-display` when `dirty === true`. Clear on save.
- **Where:** anywhere `dirty` is set/cleared
- **Effort:** ~5 lines

### 2. 0 key → reset zoom to 100%
Add `case '0':` to the keydown handler, call `zoomCentered(1)`.
- **Where:** keyboard event handler
- **Effort:** ~3 lines

### 3. M key → place at cursor, immediately enter edit mode
Currently M creates a node at screen center and requires a double-click to edit. Change: create node at current mouse position and call `startEditingMind()` immediately after.
- **Where:** `addMindmapToCanvas` call in keydown handler
- **Effort:** ~5 lines

### 4. Ctrl+D → duplicate in place
Call `copySelection()` then `pasteFromClipboard()` in sequence with a fixed offset (+20px). Currently takes 3 keystrokes (C → V → move); this makes it 1.  
**Note:** `Ctrl+D` conflicts with browser bookmarks. Use `Ctrl+Shift+D` or just `D` while objects are selected.
- **Where:** keydown handler
- **Effort:** ~15 lines

### 5. Arrow keys → nudge selected objects
`ArrowUp/Down/Left/Right` moves selected objects 1px. `Shift+Arrow` moves 10px. Guard: skip when `isEditing` is true.
- **Where:** keydown handler
- **Effort:** ~20 lines

### 6. Export selection as a new Onefile
`Ctrl+Shift+S` when objects are selected: calls `saveCanvas()` with a filtered state containing only the selected objects and their connectors. The output is a complete, standalone HTML file.
- **Where:** new branch in `saveCanvas` or a thin wrapper function
- **Effort:** ~15 lines (reuses `saveCanvas` entirely)

### 7. Cross-tab copy-paste
`internalClipboard` is a JS variable that dies when you switch tabs. Fix: when `copySelection()` runs, also write `JSON.stringify(internalClipboard)` to `localStorage['onefile_clipboard']`. When `pasteFromClipboard()` runs, if `internalClipboard` is empty, load from localStorage first. Works across any number of open Onefile tabs. The existing paste logic handles ID remapping, position offsetting, and mindmap subtrees — nothing changes there.  
**This replaces the "merge two Onefiles" feature** — Ctrl+A → Ctrl+C in one tab, Ctrl+V in another achieves the same result with more control.
- **Where:** `copySelection()`, `pasteFromClipboard()`
- **Effort:** ~10 lines

---

## Group 2 — Small new feature, self-contained (half a day each)

### 8. ~~Paste image from clipboard~~ — already working

### 9. ~~Double-click canvas to add text~~ — already working

### 10. Mindmap reflow: respect current Y order, not creation order
When the cleanup button triggers `reflowMindTree`, children are currently ordered by DOM insertion order (= creation order). Fix: sort children by their current `style.top` value before laying out. Reflow then respects the manual arrangement rather than overriding it.
- **Where:** `getChildren()` inside `reflowMindTree` — add `.sort((a,b) => parseFloat(a.style.top) - parseFloat(b.style.top))`
- **Effort:** 1 line

### 11. Ctrl+F text search
Floating search bar appears on `Ctrl+F`. Searches `textContent` of all `.text-object` and `.mind-object` elements. Matching objects get a highlight ring. `Enter` pans and zooms to the next match.
- **Where:** new `searchObjects()` function + a floating input element
- **Effort:** ~30 lines

### 12. localStorage autosave (silent crash recovery)
Every 30 seconds when `dirty === true`, serialize `captureState()` to `localStorage`. On `init()`, if a saved session exists, show a one-time dismissible toast: "Restore unsaved session?" The Ctrl+S export flow is completely unchanged — this is only a recovery net.
- **Where:** `init()`, new interval in `setupEventListeners()`
- **Effort:** ~40 lines

### 13. Mindmap subtree collapse
A small fold button (`▶`) appears on hover over any mindmap node that has children. Click to hide all descendants and show a count badge (e.g. `+5`). Click again to expand. Collapsed state stored as `data-collapsed="true"` and round-tripped in `captureObject` / `restoreObject`. `renderConnectors` skips lines to hidden nodes.
- **Where:** `createMindmapElement()`, `captureObject`, `restoreObject`, `renderConnectors`
- **Effort:** ~35 lines

---

## Group 3 — Medium effort, high payoff (a day each)

### 14. Node / sticky-note colors
A row of 6 color swatches (warm white, yellow, green, blue, pink, charcoal) appears near a selected mindmap node. Sets `background-color` and `color` on the element. Stored as a `color` field in `captureObject` / `restoreObject`.
- **Where:** new color picker UI, `createMindmapElement`, `captureObject`, `restoreObject`
- **Effort:** ~50 lines

### 15. Font size variants in text toolbar
Add S / M / L / XL buttons to the existing `.text-format-toolbar`. Applies a `font-size` value to the `.text-content` element. Stored as a `fontSize` field in `captureObject`.
- **Where:** `.text-format-toolbar` HTML, `startEditingText()`, `captureObject`, `restoreObject`
- **Effort:** ~35 lines

### 16. Text → Mindmap (paste outline, get a tree)
A dialog (or intercept paste on empty canvas) that accepts indented text. Tab or 2-space indent = one child level. Each line becomes an `addMindmapToCanvas()` call with the correct `parentId`, followed by `reflowMindTree()` to auto-position everything.
- **Where:** new `importOutlineText()` function, invoked via button or shortcut
- **Effort:** ~40 lines

### 17. Mindmap → Text (export tree as indented outline)
Select any mindmap root node. Press `Ctrl+Shift+C` (or a sidebar button). Traverses the tree recursively and copies indented plain text to clipboard. Inverse of #16. Useful when you need to write up a brainstorm.
- **Where:** new `exportMindmapText()` function
- **Effort:** ~20 lines

### 18. Canvas zones (visual backdrop areas)
New object type: `zone-object`. A large semi-transparent rounded rect with an editable title label at the top. Its body has `pointer-events: none` so clicks pass through to objects below. Move it by dragging the title bar. Lives at a z-index below all other objects. Add a button to the sidebar.
- **Where:** new `createZoneElement()` factory, `captureObject`, `restoreObject`, CSS
- **Effort:** ~45 lines

### 19. Bidirectional mindmap expansion
Root node's direct children can be on the left OR right side, like XMind / Miro. Each child node gets `data-side="left|right"`. `reflowMindTree` splits root children into two groups and mirrors the left group (childX = rootX - gap - child.offsetWidth). Bezier connector rendering flips anchor direction for left-side children (currently hardcoded right→left).  
**This is NOT the same as freeform connectors.** Freeform connectors draw arbitrary lines between any objects. This restructures how the mindmap tree itself lays out. They are separate features.  
- **Where:** `createMindmapElement()`, `reflowMindTree()`, `renderConnectors()`, node creation in keydown handler
- **Effort:** ~70 lines
- **Decision, 2026-05-07:** keep `Tab` and `+ Child` adding to the right by default. If a user manually drags a root child to the left side, clean-up should preserve that branch on the left.
- **Note:** build after #10 (reflow sort fix) is in, so the left/right reflow also benefits from Y-order sorting

### 20. Freeform connectors (any object to any object)
Hover near the edge of any canvas object → a small port dot appears. Drag from port to another object → creates a record in the `connectors[]` array (already exists in the data model). Upgrade `renderConnectors` to draw the same bezier style as mindmap connectors for these freeform connections (currently renders as straight lines).
- **Where:** `mousemove` on canvas objects, `connectors[]` (already exists), `renderConnectors()`
- **Effort:** ~70 lines
- **Decision, 2026-05-07:** this is the right direction for "break apart and connect mindmap nodes freely." A mindmap node could be detached by clearing its `data-parent-id`, then reconnected through a freeform connector. For a true mindmap relationship, a separate command should "make parent/child" and update `data-parent-id`; otherwise it is a visual connection only.
- **UX note:** keep this separate from normal mindmap branching so Onefile stays simple: mindmap tree = structured hierarchy, freeform connectors = loose visual relationships.

---

## Group 4 — Larger scope (plan before building)

### 21. Connector text labels
Each connector in `connectors[]` can hold an optional `label` string. Rendered as a `<text>` SVG element positioned at the cubic bezier midpoint. A transparent hit-target overlay catches clicks to open an inline editor. Stored in the `connectors[]` state.
- **Where:** `renderConnectors()`, `connectors[]` data model, hit-target overlay
- **Effort:** ~60 lines + UX detail
- **Dependency:** build after #20

### 22. Shapes as mindmap variant (rect / ellipse style)
Extend mindmap nodes with a `shape` property: `pill` (current default) / `rect` / `ellipse`. `rect` = `border-radius: 6px`, `ellipse` = `border-radius: 50%`. Enable free resize for non-pill shapes (remove the `height: auto` constraint). This gives PPT-style text boxes with no new object type.
- **Where:** `createMindmapElement()`, CSS, `captureObject`, `restoreObject`, a shape-picker in the toolbar
- **Effort:** ~50 lines

### 23. Canvas waypoints (presentation mode)
Press `W` to drop a numbered waypoint at the current `{panX, panY, scale}` with an optional label. In view-only mode, `←` / `→` animates smoothly between waypoints. The exported HTML file IS the presentation — no separate deck needed. Waypoints serialized in `saveCanvas()`.
- **Where:** new `waypoints[]` state var, `saveCanvas()`, `setupEventListeners()`, view-only keydown
- **Effort:** ~80 lines

---

## Not recommended

| Idea | Reason |
|---|---|
| Merge two Onefile files | Cross-tab clipboard (#7) achieves this with more flexibility and a fraction of the code. |
| Tables | Canvas metaphor breaks immediately on column resize and merging. Use zones + aligned objects instead. |
| Freehand drawing | High effort (path recording, smoothing, storage). Off-brand for a structured thinking tool. |
| Real-time collaboration | Requires a server. Breaks single-file portability. |
| Export to PNG / PDF | Needs a third-party library or complex canvas serialization. Out of scope. |
| Layers | Figma territory. Zones (#18) cover 80% of the real need with a fraction of the complexity. |

---

*Last updated: 2026-05-06. Line estimates assume vanilla JS consistent with the existing codebase style, no tests, no comments.*
