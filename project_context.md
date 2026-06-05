# Project Context & Scope: Image Color Remover

## 1. Project Overview
**Free PNG Color Remover** is a modern, fast, and feature-rich web-based tool designed to remove backgrounds, erase specific colors, and create transparent PNG images directly in the browser. 
- **Privacy First**: All processing runs locally via the HTML5 Canvas API. Images are never uploaded to any server.
- **Zero Dependencies**: The entire application is built in a single `index.html` file using Vanilla JavaScript and CSS. There is no build step (no Webpack, Vite, React, etc.).
- **Target Audience**: Web designers, meme creators, and casual users looking for a free, high-quality, non-paywalled background removal tool.

## 2. Technical Architecture
The core logic resides within the `<script>` tag in `index.html`.
- **Canvas Elements**: 
  - `srcC` (Original Canvas): Holds the loaded image and allows the user to sample colors by clicking.
  - `dstC` (Preview Canvas): Displays the processed image with backgrounds removed.
- **State Management**:
  - `targets`: An array holding the colors or "wand" seed points selected by the user.
  - `clickMode`: Dictates whether a click on the canvas samples a global "Color" or acts as a contiguous "Wand" seed.
- **Core Algorithms**:
  - `applyRemoval()`: Maps over pixels to compute the transparency mask (alpha) based on distance from the target colors (`Similarity`).
  - **Blur-Fusion (`blurFusion()`)**: A spill-removal algorithm based on Marco Forte's Approximate Fast Foreground Colour Estimation. It recomputes edge pixels to eliminate colored halos.
  - **Edge-only Mode**: Restricts removal to pixels connected to the image boundary.
  - **Auto-crop**: Scans the final alpha channel to find the bounding box and crop out empty transparent space.

## 3. Current Improvement Goals (Tasks to Solve)
The current objective is to iterate on the application by completing the following specific tasks. These tasks range from UI/UX fixes to algorithmic enhancements.

### Task 1: Canvas Responsiveness
- **Goal**: Fix canvas interaction coordinates, specifically prioritizing mobile over desktop.
- **Context**: The canvas uses CSS `object-fit: contain` inside a wrapper (`max-height: 55vh`). Currently, the coordinate mapping when a user clicks the canvas relies on `getBoundingClientRect()` relative to the DOM element's width/height. This leads to inaccurate `e.clientX` / `e.clientY` offsets on mobile or certain aspect ratios where the image doesn't fill the entire element.

### Task 2: Exclude Specific Pixels (Pen Tool)
- **Goal**: Implement a pen/brush tool with varied size options to manually exclude areas from color removal.
- **Context**: Users need a way to protect parts of the image that match the targeted background color. This requires adding a new "Exclude" mode to the UI, handling drag-to-draw on the canvas, storing an exclusion mask, and making `applyRemoval()` bypass pixels marked by this mask.

### Task 3: Enhance Removal Logic
- **Goal**: Improve the blending logic for complex elements such as fur or hair.
- **Context**: The transition from solid to transparent is currently handled via a linear ramp (`Similarity` to `Softness` thresholds). For fur, the falloff should ideally be non-linear (e.g., dense inside but fading out more transparently toward the outside). This involves adjusting the alpha calculation curve in `applyRemoval()`.

### Task 4: Fix Inconsistent Logic
- **Goal**: Address the inconsistent behavior between the "Wand" tool (Contiguous mode) and "Edge-only" exclusion.
- **Context**: The "Edge-only" mode floods inward from the image borders. The "Wand" mode floods outward from the clicked point. Currently, their interactions and propagation logic are inconsistent or can conflict when both concepts are applied. The logic needs to be harmonized (e.g., restricting Wand by Edge, or disabling Edge-only when Wand is active).

### Task 5: SEO Improvements
- **Goal**: Improve the wording and text content throughout the site for search engines.
- **Context**: The page currently has meta tags, a JSON-LD schema, and various text sections. The task involves refining the copy, headings (`<h1>`, `<h2>`), and keyword density to rank better for queries like "transparent PNG maker", "remove background from image", etc.

## Guidelines for Other AI Assistants
1. **Maintain the Single-File Structure**: Do not split the code into separate `.css` or `.js` files unless explicitly requested.
2. **Vanilla JS/CSS Only**: Stick to browser-native APIs. Do not introduce external libraries or frameworks.
3. **Performance First**: Canvas manipulation loops (like `applyRemoval` and `blurFusion`) run on the main thread. Ensure any new pixel iteration logic is highly optimized.
