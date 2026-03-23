---
name: image-optimize
description: "Reminds to optimize images added to web projects"
version: "1.0.0"
source: custom
---

# Image Optimize

When new images are added to a web project, ensure they are optimized before
committing. Unoptimized images bloat the bundle and hurt performance.

## Workflow

1. **Detect new images.** When you write an image file or notice the user has
   added images, check:
   - What format are they? (png, jpg, jpeg, webp, svg)
   - What size are they? (run `ls -lh` on the file)
   - Where are they? (which directory)

2. **Check for an optimize script.** Look for:
   - `npm run optimize-images` (portfolio uses this)
   - A `scripts/optimize-images.js` or similar file
   - Sharp, imagemin, or other image optimization tools in package.json

3. **If an optimize script exists:** Run it.
   ```
   npm run optimize-images
   ```
   Report what was optimized and the size savings.

4. **If no optimize script exists:** Flag the issue to the user:
   - Report the image file size
   - Suggest converting to WebP if it's PNG/JPG (WebP is typically 30-50%
     smaller)
   - Suggest appropriate dimensions (most portfolio images don't need to be
     wider than 1200px)
   - Offer to convert using sharp or sips (macOS built-in):
     ```
     sips -s format webp -Z 1200 input.png --out output.webp
     ```

5. **Before committing:** Double-check that no unoptimized originals are being
   staged alongside their optimized versions. Only commit the optimized files
   unless the user explicitly wants both.

## Size Guidelines

| Use Case | Max Width | Format | Target Size |
|----------|-----------|--------|-------------|
| Hero/cover image | 1200px | WebP | < 100KB |
| Project card thumbnail | 800px | WebP | < 60KB |
| Icon/logo | 512px | WebP or SVG | < 20KB |
| Profile picture | 200px | WebP | < 15KB |

## Edge Cases

- If the image is already WebP and under the target size, skip optimization
  and confirm it's good.
- If the image is an SVG, do not convert it — SVGs are already optimized for
  web. Only flag if the SVG is unusually large (> 50KB).
- If the user is in a rush ("just commit it"), flag the optimization but
  don't block the commit. Add a note that optimization is pending.
