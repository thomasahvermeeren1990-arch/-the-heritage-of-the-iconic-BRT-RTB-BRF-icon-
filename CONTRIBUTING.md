# Contributing to the BRT/RTB/BRF heritage icon

Thank you for contributing. This document explains how to produce, test and commit artwork assets that conform to the project's BRAND GUIDELINES.

Please follow these rules for any contribution:

1. Use the vector source (assets/source.svg) as the canonical source of truth. Do not rasterize and then edit — make edits in the SVG.
2. Official exports must use the exact color palette listed in BRAND-GUIDELINES.md. The canonical blue used in the SVG is RGB(0,0,255). Use sRGB color space for all exports.
3. Official export sizes: 16000×16000 px (official), and provide derivative exports at 8000×8000, 4000×4000 and 2000×2000 for convenience.
4. Background: official PNG exports use a white background (RGB 255,255,255). If you provide transparent variants, mark them clearly.
5. Anti‑aliasing and pixel accuracy:
   - To avoid introducing new intermediate colors, prefer exporting from the SVG at integer scales so that vector node coordinates map to exact pixel boundaries when possible.
   - If you must rasterize with anti‑aliasing, create an indexed version afterwards and verify the palette (see palette instructions below).
6. Indexed/7‑color PNG: produce an 8‑bit indexed PNG with exactly the 7 official colors for the "indexed" variant. Do not introduce additional palette entries.
7. Tools: The project requests (as a preference) that personal, offline edits use MS Paint or MS Paint 3D; contributors may use any tools to produce official assets. When committing assets, include the exact command(s) you used to create them.

Palette and palette file
- The repository includes an example palette file at assets/palette-7.png (7-pixel image with each pixel set to one of the required colors). Use this to remap colors when producing indexed PNGs.

Suggested commands (examples)
- Inkscape (recommended for vector fidelity):
  inkscape assets/source.svg --export-filename=assets/16000x16000.png --export-width=16000 --export-height=16000 --export-background="#ffffff" --export-background-opacity=1

- ImageMagick (for remapping to a fixed palette without dithering):
  # create palette image (one row with the 7 colors) if not already present
  convert -size 7x1 xc:"rgb(128,128,128)" xc:"rgb(255,0,0)" xc:"rgb(0,255,0)" xc:"rgb(0,0,255)" xc:"rgb(255,255,255)" xc:"rgb(255,255,0)" xc:"rgb(0,0,0)" assets/palette-7.png

  # export a 16k PNG from SVG (Inkscape recommended) and then remap to palette with no dithering
  convert assets/16000x16000.png -dither None -remap assets/palette-7.png assets/indexed-7color.png

- rsvg-convert (alternative to Inkscape):
  rsvg-convert -w 16000 -h 16000 -b white assets/source.svg -o assets/16000x16000.png

Quality checks
- Verify the final indexed PNG contains only the 7 colors. With ImageMagick:
  convert assets/indexed-7color.png -unique-colors txt:- | sed -n '1p;2,999p'

- Inspect edges at 100% zoom to ensure no unintended intermediate colors. If anti‑aliasing introduced gray pixels, consider adjusting vector node placement or re-exporting at a size that aligns nodes to integer pixels.

Committing
- When you commit exported assets, add a short commit message like "Add 16k and indexed exports" and include the commands you used in the commit message or in a separate ASSET_GENERATION.md file.

If you want, I can generate working export commands tailored to your environment (Linux / macOS / Windows) or produce the PNG exports for you and commit them — tell me which you prefer.