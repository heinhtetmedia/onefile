# Onefile

**Portable Whiteboard, in a Single File.**

Onefile is a tiny infinite-canvas whiteboard that lives entirely inside one HTML file. No install, no account, no server, and no build step. Open `index.html` in a browser and start thinking.

Live version: https://heinhtetmedia.github.io/onefile/

## What It Does

- Infinite canvas with pan, zoom, fit-to-content, drag, resize, and grouping
- Text boxes with direct paste from outside sources
- Image paste/upload with embedded image data and image-only position lock
- Mindmap nodes with child/sibling creation, append-to-bottom sibling placement, auto-connectors, clean-up layout, subtree copy/cut/paste, and mobile-friendly action buttons
- Mobile touch support for pan, pinch zoom, double-tap editing, and drag-select mode
- Undo/redo, view-only mode, and a shortcuts/help panel
- Save/export to a new standalone `.html` file with the current canvas embedded
- Unsaved-change indicator beside the document title

## Use It

1. Open the live version or download `index.html`.
2. Add text, images, or mindmap nodes.
3. Use **Ctrl/⌘ + S** or the save button to export your board as a standalone HTML file.
4. Reopen that saved HTML file anytime. It carries the app and your content together.

## Shortcuts

| Shortcut | Action |
| --- | --- |
| `T` | Add text |
| `I` | Add image |
| `M` | Add mindmap root |
| `Tab` | Add mindmap child |
| `Enter` | Add mindmap sibling |
| `Shift + Enter` | Line break while editing a mindmap node |
| `Shift + click` | Multi-select |
| `Shift + drag` | Drag-select on desktop |
| `Space + drag` | Temporary pan |
| `Ctrl/⌘ + S` | Save Onefile |
| `Ctrl/⌘ + Z` / `Ctrl/⌘ + Y` | Undo / redo |
| `Ctrl/⌘ + C` / `X` / `V` | Copy / cut / paste |
| `Ctrl/⌘ + A` | Select all |
| `Delete` | Delete selected |
| `Ctrl/⌘ + G` | Group / ungroup |
| `1` | Fit to content |

## Latest Updates

- Mobile drag-select now tracks the actual touch position.
- Fit-to-content uses the stable screen viewport on mobile.
- Mindmap sibling creation appends below existing siblings to avoid overlap.
- Image selection has a position lock beside delete, with locked/unlocked icon states.
- The add-text dialog now uses the same softer neutral UI style as the rest of the app.

## Repository

The published app intentionally keeps the repository small: `index.html`, `README.md`, and `LICENSE`.

## License

MIT. Created by [Hein Htet](https://www.linkedin.com/in/heinhtetthemarketer/).
