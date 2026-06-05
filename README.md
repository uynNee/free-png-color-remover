# Free PNG Color Remover

A simple, private tool to erase background colors and make transparent PNGs directly in your browser. 

It is designed to run entirely on your own device—no image uploads, no server processing, and no accounts required.

## Core Features

- **Multiple Erasing Modes**: 
  - **Color Picker**: Erase a selected color wherever it appears in the image.
  - **Magic Wand**: Erase only the connected region of the color you click (useful when you want to keep matching colors elsewhere).
  - **Erase & Protect Brush**: Draw directly on your image to manually wipe away areas or protect fine details.
- **Fine-Tuning Controls**:
  - **Similarity & Softness**: Fine-tune how much color is removed and soften the edges so cutouts blend smoothly onto new backgrounds without colored halos.
  - **Edge-Only Mode**: Clean outer background colors while keeping the identical colors safe inside your subject (like keeping a white shirt on a white background).
- **Auto-Crop**: Automatically crop the final transparent image to its exact boundaries.
- **Offline First**: Runs completely offline. You can save the website to your computer as a single HTML file and use it anywhere.

## How It Works Under the Hood

For those interested in what happens in the browser:

1. **Local Canvas Editing**: All changes are calculated in your browser using standard 2D Canvas contexts, ensuring no data ever leaves your machine.
2. **Edge Softening**: Instead of a simple harsh cutout, the tool estimates the local background colors around edges to remove background color bleeding (halos), creating a clean, soft transition.
3. **Guided Flood Fill (Wand Barriers)**: The Keep/Protect mask you draw acts as a physical boundary. If you have gaps in your lineart, you can brush over them to prevent the Magic Wand or Edge-only fill from leaking through.
4. **History Log**: The brush masking mode records a simple 15-step transaction stack, allowing you to undo and redo manual brush strokes instantly.

## Getting Started

1. Open [index.html](file:///d:/uynne/Image%20Color%20Remover/index.html) in any modern browser.
2. Drop or upload your image.
3. Click the color you want to remove on the image preview, or switch to **Wand** or **Brush** mode to edit manually.
4. Use the sliders to adjust how much color is erased and how soft the borders are.
5. Click **Save Transparent PNG** to download.

## License

Copyright (c) 2026 uynNee

This work is licensed under the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0). Under this license, you are free to share and adapt the code for non-commercial purposes, provided you give appropriate credit.
