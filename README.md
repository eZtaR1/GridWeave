# GridWeave
A browser-based tool to convert an input image into layered SVG files suitable for pen plotting. It creates a halftone-like effect using circles arranged on distorted grids, with configurable colors, styles, and distortion methods.

This tool is inspired by generative plotter art styles, such as those seen by artists like pavlovpulus, using multiple coloured pens and distorted grids based on underlying images.

## Features
![Screenshot 2025-04-08 at 21-31-36 Flowing Halftone SVG Generator](https://github.com/user-attachments/assets/212263fe-500b-4721-a2e1-a745e7bc8d57)

* **Image Upload:** Process your own images directly in the browser.
* **Layered SVG Output:** Generates 3 separate SVG files, one for each color layer, ideal for multi-pen plotting.
    * Layer 1: Main Grid - Color 1 (Alternating)
    * Layer 2: Main Grid - Color 2 (Alternating)
    * Layer 3: Offset Grid - Single Color
* **Brightness Mapping:** Circle diameters are varied based on the brightness of the underlying grayscale image (darker areas produce larger circles by default).
* **Configurable Distortion:** Choose between two distortion modes:
    * **Wave Distortion:** A uniform `sin`/`cos` based wave pattern applied to the grid coordinates.
    * **Image-Based Flow:** Distorts the grid based on the calculated brightness gradient of the image, causing grid lines to "flow" along image contours. (Requires image processing time).
* **Customizable Parameters:** Adjust settings via sliders and inputs:
    * Grid Spacing
    * Min/Max Circle Diameter
    * Offset Grid distance (X and Y)
    * Distortion Amount (overall strength)
    * Wave Scale (for Wave Distortion mode)
* **Color Selection:** Choose custom colors for the three layers using color pickers.
* **Fill/Stroke Option:** Output circles as solid fills or as outlines (strokes) with configurable stroke width.
* **Millimeter Output:** Specify the desired physical width of the output SVG in millimeters (mm). Height is calculated automatically.
* **Live Previews:** See previews of each individual SVG layer and a combined overlay.
* **Download Links:** Easily download the generated individual and combined SVG files.

## How to Use

1.  **Save:** Save the HTML code provided into a file named `gridweave.html` or whatever.
2.  **Open:** Open this HTML file in a modern web browser (like Chrome, Firefox, Edge).
3.  **Upload:** Click the "Choose File" button and select your desired source image (JPEG, PNG, GIF, etc.).
4.  **Process:** Wait a few moments. The tool will display a preview, convert the image to grayscale, and (if gradient distortion is available) calculate the image's flow field. A status message will indicate when it's "Image ready."
5.  **Configure:** Adjust the settings in the control panels:
    * Set the desired `Output Width (mm)`.
    * Fine-tune grid spacing, circle sizes, offsets.
    * Select distortion type ("Use Image-Based Flow" checkbox) and amount. Adjust Wave Scale if using wave distortion.
    * Choose your desired colors.
    * Select fill or stroke output and set stroke width if needed.
6.  **Generate:** Click the "Generate SVGs" button.
7.  **Preview & Download:** Previews for each layer and the combined output will appear below the controls. Use the download links to save the SVG files.

## Controls Overview

* **Output Width (mm):** Desired physical width of the final SVG output. Height is calculated automatically based on image aspect ratio.
* **Grid Spacing (px):** Distance between points on the base pixel grid applied to the source image. Smaller values mean denser grids.
* **Max/Min Circle Diameter (px):** Range of circle sizes (relative to the pixel grid) mapped from image brightness (0-255). By default, black (0) maps to Max Diameter, white (255) maps to Min Diameter.
* **Offset Grid X/Y (px):** How much to shift the base position of the third (Offset Color) grid relative to the main grid, before distortion is applied.
* **Use Image-Based Flow:** Checkbox to enable distortion based on image brightness gradients. This makes the grid appear to follow contours in the image. *This is only enabled after the image is processed and the gradient field is calculated.*
* **Distortion Amount (px):** A multiplier controlling the overall strength of the selected distortion effect. Higher values mean more displacement.
* **Wave Scale:** Adjusts the frequency/scale of the waves for the **Wave Distortion** mode. *This slider has no effect if "Use Image-Based Flow" is checked.*
* **Main Color 1 / Main Color 2:** Colors used for the alternating circles on the primary grid layer.
* **Offset Color:** Color used for the circles on the secondary, offset grid layer.
* **Use Outlines (Strokes):** Checkbox to draw circle outlines instead of solid filled circles.
* **Stroke Width (px):** Sets the width of the outlines if "Use Outlines" is checked. This width is relative to the pixel grid (`viewBox`) of the SVG.

## Output Files

* **Layered SVGs:** The primary output is 3 separate SVG files, named using the layer number and color (e.g., `halftone-layer-1-#FFFF00.svg`). Each file contains only the circles for that specific color.
* **Combined SVG:** A fourth SVG file (`halftone-combined.svg`) is also generated, containing all three layers overlaid, useful for a complete digital preview.
* **Units:** All generated SVGs have their main `width` and `height` attributes set in `mm`. The internal `viewBox` is based on the source image's pixel dimensions.

## Pen Plotter Workflow

To use the generated SVGs with a pen plotter:

1.  You need software to convert SVG to G-code suitable for your plotter (e.g., GRBL, HPGL).
2.  **Inkscape (Recommended):**
    * Install Inkscape (free vector editor).
    * Install a G-code plotter extension compatible with your plotter (e.g., Inkscape-Plot, AxiDraw plugin).
    * Open one **individual layer SVG** file (e.g., the Yellow layer).
    * Verify Document Properties (`File > Document Properties`) are in `mm`.
    * Select All (`Ctrl+A`).
    * Convert circles to paths (`Path > Object to Path`).
    * Open your G-code extension (`Extensions` menu).
    * Configure plotter settings (Pen Up/Down commands, speeds, etc.).
    * Export the G-code file.
    * **Repeat** this process for the other two individual layer SVG files, ensuring you use the correct pen for each corresponding G-code file during plotting.

## Technical Notes

* Image processing (grayscale, pixel access, gradient calculation) uses the HTML Canvas 2D API.
* Gradient calculation uses a simple central difference method.
* Gradient-based distortion displaces points perpendicular to the local gradient vector.
* SVG files are generated as text strings containing `<circle>` elements within `<g>` tags.

---
