# Onefile Project Progress

Last updated: 2026-05-07

## Current Status

The active app source is `index.html`. The app is still a single portable HTML file with no build step or dependencies.

Recent working areas:
- Mindmap action bar is now the compact neutral button bar: `+ Child`, `+ Sibling`, `Clean up`.
- `+ Sibling` is hidden on the top parent/root node.
- Action bar stays aligned while typing/editing mindmap nodes.
- `+ Child` places new children below existing children when needed.
- Ctrl/Cmd click-drag selection conflict was removed. Multi-select and marquee now use Shift.
- Text paste is plain text, and external text can be pasted directly onto the canvas.
- Text boxes can be resized from the left/right border area, including pasted text.
- Onefile object copy/paste avoids duplicate overlapping copies.
- Cross-document Onefile copy/paste now writes object payloads through the native copy/cut event where available, with localStorage fallback.
- Pasted external text/images and Onefile objects now anchor their top edge at about 30% from the top of the viewport.
- Mindmap connector lines are layered above images but below nodes/text.
- Left toolbar is centered against the full viewport height.
- Mobile supports double-tap edit for text and mindmap nodes, plus a bottom-left touch select-mode button for drag-select.
- Shortcuts popup is grouped, wider on desktop, and collapses to one column on mobile.
- Tooltips use simple bracketed shortcut text like `Add text (T)` after rejecting the keycap-style tooltip treatment as visually noisy.

## Deferred Decisions

Two-sided mindmap branches:
- Possible, but treat as a feature rather than a quick fix.
- Default behavior stays simple: `Tab` and `+ Child` add to the right.
- If a user manually drags a root child to the left, future clean-up should preserve that branch on the left.
- Implementation likely needs a first-level branch side field or a position-based side inference, plus reflow and connector changes.

Break-apart / free-connect mindmap nodes:
- Possible, but should be separate from normal mindmap hierarchy.
- Detaching a node could clear `data-parent-id`.
- Freeform connectors can visually connect detached nodes or any object to any object.
- A separate "make parent/child" command would be needed if a visual connection should become a real mindmap hierarchy relationship.

## Save-Cycle Reminder

When adding functions, update `FN_NAMES`.
When adding new constants or state, update the save serialization in `saveCanvas`.
When adding DOM elements, update `buildStaticBodyHTML`.
