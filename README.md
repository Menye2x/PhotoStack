# Photo Stack

**Photo Stack** is a browser-based image composition tool for combining multiple photos into a single clean composite canvas and exporting the result as **PNG, WebP, or JPG**.

The app is designed as a lightweight, client-side workflow: photos are decoded and processed in the browser, the composition is rendered to an HTML canvas, and the final image can be exported without requiring a server-side image-processing backend.

## Concept

Photo Stack turns a batch of imported images into one structured composition using a flexible row/column grid. The workflow is intentionally direct:

1. Import photos.
2. Choose or adjust an arrangement.
3. Fine-tune spacing and canvas padding.
4. Choose how each image fits inside its cell.
5. Preview the composition at different zoom levels.
6. Select the output format and export settings.
7. Export the final composite image.

The composition is generated from the original decoded image dimensions. Grid capacity can be larger than the number of photos, but the default arrangement logic is intended to keep unused space practical and predictable.

## Main Features

### Image import

- Drag-and-drop support.
- File picker support.
- Keyboard shortcut: `Ctrl/Cmd + O`.
- Up to **20 photos** per composition.
- Maximum source file size: **30 MB** per file.
- Supported formats:
  - JPG / JPEG
  - PNG
  - WebP
  - GIF (static frame)
  - AVIF
  - HEIC / HEIF when the browser provides a native decoder
- Image orientation is respected when the browser supports `createImageBitmap()` with image orientation handling.
- Imported photos are displayed in a reorderable list.
- Photos can be moved up/down or reordered by dragging.
- Removing a photo does not delete the original file from the user's device.

### Automatic grid and presets

The grid system is designed to make common arrangements fast to reach.

- Presets are generated dynamically from the current number of imported photos.
- `Auto` selects the default arrangement for the current photo count.
- Additional practical row/column arrangements can be selected directly from the preset area.
- Rows and Columns are linked so manual changes adjust the complementary dimension to keep the arrangement valid for the current number of photos.
- Photo placement starts at the **top-left** and fills **left-to-right, then top-to-bottom**.
- When a grid contains unused cells, empty cells remain after the imported photos rather than being inserted before them.

### Layout controls

- **Rows**: controls the number of horizontal rows.
- **Columns**: controls the number of vertical columns.
- **Gap**: adjusts spacing between cells.
- **Canvas padding**: adjusts the outer margin around the composition.
- **Contain**: keeps the complete source image visible inside its cell.
- **Cover**: fills the cell and crops overflow when necessary.
- Canvas backgrounds:
  - White
  - Transparent
  - Custom color

### Preview and zoom

The preview area is isolated from the rest of the page and supports controlled navigation of large compositions.

- `Fit` returns the preview to the fitted view.
- `−` / `+` provide controlled zoom steps.
- The zoom readout displays the current zoom percentage.
- When the composition becomes larger than the preview viewport, horizontal and vertical scrolling are available.
- The preview also supports grab/pan interaction for navigating zoomed content.
- Zoom state and navigation remain contained inside the preview area instead of scaling or moving the entire page.
- A **Zoom Map** is available when the image is zoomed to provide a compact overview and navigation aid.
- The Zoom Map can be shown/hidden by the user and supports pointer-based navigation.

### Preview background

The `Outer` control changes only the background surrounding the collage inside the preview.

- Light
- Dark

This is independent from the exported canvas background.

### Export

Three output formats are available:

- **PNG** — lossless image output with transparency support.
- **WebP** — compressed output with configurable encoder quality.
- **JPG** — lossy output with configurable encoder quality; transparent canvas areas are composited onto a solid background before encoding.

Export settings include:

- Filename.
- Output format.
- Encoder quality for JPG/WebP.
- Output scaling for JPG/WebP.
- Reduce scaling: **15–100%**.
- Enhance scaling: **100–200%**.
- Enhance mode applies additional Pica unsharp sharpening.
- Final-resolution display.
- Estimated export size.

Encoder quality controls compression quality and is separate from output scaling. Scaling changes the exported pixel dimensions; encoder quality changes how the selected JPG/WebP codec compresses the resulting pixels.

### High-quality scaling

**Pica.js 10.0.3** is used for export scaling when the selected output size differs from the rendered composition.

- Reduce uses high-quality downsampling.
- Enhance performs high-quality upscaling and applies the configured unsharp sharpening step.
- PNG export keeps the original composition resolution because the current PNG path does not use the JPG/WebP scaling controls.

### Estimated file size

The estimated export size is generated by encoding the current composition in the selected output format and settings. This means the estimate is based on an actual browser-generated export blob rather than only a mathematical approximation.

## UI and Visual Design

The interface uses a compact three-area desktop workspace:

- **Imported Photos** sidebar
- **Canvas Preview** workspace
- **Layout / Export** inspector

The visual system includes:

- Light and dark themes.
- Frosted/soft UI surfaces.
- Consistent border radius and spacing tokens.
- Responsive layouts for tablet and mobile widths.
- Keyboard focus states.
- Reduced-motion support.
- Toast notifications for export and input errors.
- Confirmation modal for destructive photo removal actions.

## Privacy and Processing Model

Photo Stack is intended to work as a browser-only processing tool.

- Source photos are processed on the current device.
- No server-side image upload is required for the core workflow.
- Export files are generated in the browser using Canvas APIs.
- The application can continue to provide its core functionality without a backend; external library loading may still depend on network availability.

## Technical Stack

- Single-file HTML application.
- HTML + CSS + vanilla JavaScript.
- HTML Canvas for composition rendering.
- Canvas `toBlob()` for image export.
- **Pica.js 10.0.3** for high-quality export scaling.
- No framework or build system is required for the current prototype.

## Current Project Structure

```text
.
├── collager photos.html
├── README.md
└── .gitignore
```

The main application is intentionally kept as a single HTML file so it can be opened directly in a modern browser and hosted easily on static hosting services such as GitHub Pages.

## Running Locally

No installation step is required for the current prototype.

Open the main HTML file in a modern browser:

```text
collager photos.html
```

For the most consistent behavior, use a current Chromium-based browser, Firefox, or Safari with Canvas, `createImageBitmap()`, `toBlob()`, drag-and-drop, and pointer events supported.

## External Dependency

The application currently loads Pica.js 10.0.3 from jsDelivr:

```html
<script src="https://cdn.jsdelivr.net/npm/pica@10.0.3/dist/pica.min.js"></script>
```

The main application itself has no package manager or build dependency.

## Design Principles

Photo Stack follows a few simple product principles:

- **Fast first result** — importing photos should immediately produce a usable composition.
- **Predictable arrangement** — the grid should not unexpectedly introduce large unused areas before the imported photos.
- **Local processing** — the core image workflow should remain client-side.
- **Non-destructive editing** — layout operations do not modify the original files.
- **Clear separation of concerns** — layout, preview navigation, scaling, and encoding quality are independent controls.
- **Responsive interaction** — zoom, pan, scrolling, and the Zoom Map should behave as navigation of the preview, not as scaling of the entire document.

## Status

**Prototype v1.10 — ACC workflow**

This README documents the current accepted behavior of the prototype, including the grid/preset workflow, contained preview zoom behavior, and browser-based export pipeline.
