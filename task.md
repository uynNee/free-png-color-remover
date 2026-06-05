# Image Color Remover — Task Tracker

---

## 1. Canvas Layout: Mobile & Desktop Sizing
**Status:** Done.

**What was done:**
- `[x]` Desktop: Wrap workspace card contents in `.workspace-sticky-wrapper`.
- `[x]` Desktop: Update `.panels` grid `align-items` to `stretch` (or remove `start`) so the workspace card stretches to the height of the controls.
- `[x]` Desktop: Remove sticky positioning from the controls card.
- `[x]` Desktop: Apply `position: sticky; top: 32px;` to `.workspace-sticky-wrapper`.
- `[x]` Mobile: Update workspace card to be sticky at `top: 0` with `height: 30vh`.
- `[x]` Mobile: Ensure the `.card-header` (with Result/Original toggle) remains visible and compact.
- `[x]` Mobile: Make `.canvas-wrap` scale correctly within the remaining space of the 30vh sticky card.
- `[x]` Mobile: Hide `.hint` inside the workspace card.
- `[x]` Mobile: Remove previous sticky/glassmorphism overrides from the controls card.

## Hotfix: Protect Mask as Boundary Barrier
**Status:** Done.

**What was done:**
- `[x]` Updated Wand tool flood fill to treat Protect/Keep pixels (`brushMask[p] === 1`) as an authoritative boundary, preventing propagation.
- `[x]` Updated Edge-only mode flood fill to similarly respect Protect pixels as hard barriers.
- `[x]` This ensures users can draw a Protect mask over lineart gaps to successfully block edge-only background removal from leaking inside.

---

## 2. Exclude Specific Pixels — Pen/Brush Tool
**Problem:** No way to protect specific pixels from removal or manually erase specific areas. Not implemented.

**Status:** Done.

**What was done:**
- `[x]` Added Brush Editor modal with Toggle buttons: Mask Edit Mode (Keep / Erase)
- `[x]` Implemented BrushDrawing and Mask-Drawing Logic on a per-pixel basis on the Preview Layer
- `[x]` Made sure that any Mask draws are persisted correctly during Undo/Redo cycles and on Canvas Resize
- `[x]` Updated the Final MattingLogic to correctly interpret the new Mask Data and apply the required alpha values, effectively keeping the masked areas or erasing them respectively.

**Goal:** Brush/pen tool with adjustable size that either:
- **Protects** pixels (marks them as keep — excluded from removal), or
- **Erases** pixels manually (paints transparency)

Varied brush size. Works on the preview canvas.

---

## 3. Enhance Removal Logic for Soft/Dense Edges (Fur, Hair)
**Problem:** `chamfer()` distance transform is defined (line ~1482) but **never called** in `applyRemoval`. Current alpha matte is a flat coverage ramp (lo → hi) with no spatial gradient. Fur and hair need: fully opaque at the dense interior, progressively more transparent toward the outer fringe — not a hard threshold.

**Goal:** Use chamfer distance (or equivalent) to weight alpha by distance from the solid subject core. Outer fringe pixels get lower alpha than interior pixels even if coverage score is the same.

**Where to look:**
- `chamfer()` unused (line ~1482)
- `applyRemoval()` → alpha matte build step 3 (line ~1687)
- `radiusFor()` / `blurFusion()` also relevant

---

## 4. Wand Tool + Edge-Only Mode Inconsistency
**Problem:** `edgeonly.checked` only gates the color-target (`colorIdx`) path (line ~1663). Wand/seed targets skip that branch entirely — toggling "Edge-only mode" does nothing when using Wand mode. Mixed-mode (wand + color targets together) has unpredictable owner assignment.

**Goal:** Edge-only behavior should apply consistently regardless of mode. Define clear interaction rules when both wand and color targets coexist.

**Where to look:**
- `applyRemoval()` seed flood (line ~1619) — no `edgeonly` check
- `applyRemoval()` color path edge-only branch (line ~1663)
- `edgeonly` toggle listener (line ~1809)

---

## 5. UI Overhaul & Color Wheel Integration
**Status:** Done.

**What was done:**
- Remove `swatches` and default `input[type="color"]`.
- Add `iro.js` via CDN to embed a responsive, beautiful color wheel inside the controls panel.
- Completely redesign the CSS for `.panels`, `.panels-canvases`, `.card` and controls to create a sleek, premium Layout (Sidebar on Desktop, polished Bottom Sheet layout on Mobile).
- Implement beautiful micro-interactions, custom scrollbars, and refined sliders.
