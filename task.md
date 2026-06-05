# Image Color Remover — Task Tracker

---

## 1. Canvas Layout: Mobile & Desktop Sizing
**Status:** Done.

**What was done:**
- Desktop: Restored individual cards with shadows for Canvases. Made the Controls column sticky instead of the canvases column, allowing smooth natural scrolling of long images while controls stay accessible.
- Mobile: Used `display: contents` on `.panels-canvases` to let Preview become a sticky sibling to Controls. Visually linked Original and Preview cards using flush margins (`margin-bottom: -32px`) and shared border-radius, creating a split-card effect when scrolling.
- Restored checkerboard background on mobile preview by removing opaque `#dstWrap` override.

---

## 2. Exclude Specific Pixels — Pen/Brush Tool
**Problem:** No way to protect specific pixels from removal or manually erase specific areas. Not implemented.

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
