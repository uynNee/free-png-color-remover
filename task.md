# Image Color Remover — Task Tracker

---

## 1. Canvas Layout: Mobile & Desktop Sizing
**Problem:** On mobile, both `.card` panels stack vertically (1-column grid) but each canvas uses `max-height: 55vh`. Original + Preview together consume 110vh+ before controls render — forces scroll, hard to see what's happening. On desktop the image can feel too small if the image is portrait.

**[NEW] Status:** Partially done — sizing fixed, visual polish still needed.

**Goal:** Canvas fills full card width on mobile (natural aspect ratio, no artificial height cap that wastes space). Desktop: big enough to work with but doesn't dominate. No content cut off above the fold.

**[NEW] What was done:**
- Removed `max-height: 55vh` cap on mobile; canvas now fills card width at natural aspect ratio
- Desktop bumped to `60vh`, then restructured to full `100vh` sticky left column
- HTML restructured: original + preview canvases in `.panels-canvases` wrapper (left col), controls in separate card (right col)
- Mobile: preview card is `position: sticky; top: 0` so it pins while controls scroll beneath

**[NEW] Remaining visual issues:**
- Desktop unified card column (`.panels-canvases`) — the two stacked sections (Original / Preview) may still look off depending on image sizes and proportions. Inner cards have `padding: 20px 24px`, divider line between them. No individual shadows.
- Mobile: 3 cards now visible (Original, sticky Preview, Controls). Can feel heavy/disconnected. Original card has full shadow/border weight. No visual cue linking canvas cards together.
- Checkerboard background on `.canvas-wrap` used `transparent` gaps — content behind sticky showed through. Fixed with `background-color: #E8EDF3` as base + solid white on `#dstWrap` mobile sticky.

**Where to look:**
- `.canvas-wrap` → `max-height: 55vh` (line ~372)
- `canvas` → `max-height: 55vh; max-width: 100%` (line ~394)
- `@media (max-width: 900px)` block (line ~1019)
- [NEW]:
- `.panels-canvases` CSS — global + `@media (min-width: 900px)` block
- `@media (max-width: 900px)` — `.panels-canvases .card:last-child` sticky rules
- `#dstWrap` mobile overrides

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
