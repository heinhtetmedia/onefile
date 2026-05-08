# Onefile

**Portable Whiteboard, in a Single File.**

Onefile is a tiny infinite-canvas whiteboard that lives entirely inside one HTML file. No install, no account, no server, and no build step. Open `index.html` in a browser and start thinking.

Live version: https://heinhtetmedia.github.io/onefile/

## What It Does

- Infinite canvas with pan, zoom, fit-to-content, drag, resize, and grouping
- Text boxes with direct paste from outside sources and edit-mode formatting
- Image paste/upload with embedded image data, image-only position lock, visible image copy/download/delete actions
- Mindmap nodes with child/sibling creation, two-sided clean-up layout, subtree detach/reconnect, bullet outline paste/copy, and mobile-friendly action buttons
- Mobile touch support for pan, pinch zoom, double-tap or long-press editing, pencil edit, and guided drag-select mode
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
| `S` / `M` / `L` buttons | Format selected text size while editing |
| `B` / `I` / `U` buttons | Bold, italic, or underline while editing |
| `Shift + click` | Multi-select |
| `Shift + drag` | Drag-select on desktop |
| `Space + drag` | Temporary pan |
| `Ctrl/⌘ + S` | Save Onefile |
| `Ctrl/⌘ + Z` / `Ctrl/⌘ + Y` | Undo / redo |
| `Ctrl/⌘ + C` / `X` / `V` | Copy / cut / paste |
| `Ctrl/⌘ + C` with one image selected | Copy the image bitmap/data |
| `Ctrl/⌘ + A` | Select all |
| `Delete` | Delete selected |
| `Ctrl/⌘ + G` | Group / ungroup |
| `1` | Fit to content |

## Latest Updates

- Saved files include a no-JavaScript fallback message for iPhone/iPad Quick Look limitations.
- Text and mindmap nodes share an edit-mode formatting toolbar with S/M/L and B/I/U.
- Mobile editing is discoverable with long-press and a pencil edit button.
- Bullet or numbered outlines can be pasted as mindmaps, and mindmaps can be copied back as outline text.
- Mindmap subtrees can be detached/reconnected with node handles, and clean-up preserves left/right branches.
- Image selection has copy/download/position-lock/delete actions; single-image keyboard copy copies the image data.
- Locked images stay fixed while drag gestures over them pan the canvas.

## iOS Note

Saved Onefile HTML files need a browser that can run JavaScript. iPhone/iPad Files preview or Quick Look may show the file without running it properly. For reopening saved Onefile files, use a computer, Android browser, or another browser environment that supports standalone HTML files with JavaScript.

## Repository

The published app intentionally keeps the repository small: `index.html`, `README.md`, and `LICENSE`.

## License

MIT. Created by [Hein Htet](https://www.linkedin.com/in/heinhtetthemarketer/).
