# Free PNG Color Remover

A modern, fast, and feature-rich web-based tool to remove backgrounds, erase specific colors, and create transparent PNG images directly in your browser. 

Designed with a sleek, premium, and responsive user interface, it runs entirely client-side for absolute privacy and maximum speed.

## Features

- **Multi-Mode Color Erasing**:
  - **Global Mode**: Erase the target color wherever it appears in the image.
  - **Contiguous (Wand) Mode**: Erase only the connected region of the selected color (like a magic wand tool).
- **Tolerance & Edge Tuning**:
  - **Similarity**: Adjust color distance matching threshold.
  - **Softness**: Control edge feathering/transparency falloff for a smooth transition.
  - **Edge-Only (Border Connected)**: Clean outer backgrounds while preserving interior colors.
- **Autocrop**: Automatically crop the resulting transparent PNG to its exact non-transparent bounding box.
- **Interactive Staging**: Add multiple target colors to erase several background elements at once.
- **Privacy First**: All processing is performed locally in your browser using canvas APIs; your images are never uploaded to any server.

## Getting Started

1. Open `index.html` in any modern web browser.
2. Drag & drop or upload your image.
3. Click on the image preview to select the color you want to remove, or click **+ Add color** with the color picker.
4. Adjust the **Similarity** and **Softness** sliders to fine-tune the removal.
5. Click **Download** to save your transparent PNG!

## License

Copyright (c) 2026 uynNee

This work is licensed under the Creative Commons Attribution-NonCommercial 4.0 International License. 
To view a copy of this license, visit http://creativecommons.org/licenses/by-nc/4.0/

Under this license, others are free to:
- Share: Copy and redistribute the material in any medium or format.
- Adapt: Remix, transform, and build upon the material.

Under the following terms:
- Attribution: You must give appropriate credit, provide a link to the license, and indicate if changes were made.
- NonCommercial: You may not use the material for commercial purposes (DO NOT SELL).
