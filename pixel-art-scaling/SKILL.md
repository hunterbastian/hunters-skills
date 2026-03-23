---
name: pixel-art-scaling
description: "Ensures crisp pixel art rendering in 2D game projects"
version: "1.0.0"
source: custom
---

# Pixel Art Scaling

When working on a pixel art game, ensure sprites render crisp without
subpixel blurring. Blurry pixel art is the #1 visual bug in retro-style
games.

## Workflow

1. **Detect pixel art context.** Look for signs this is a pixel art project:
   - Small sprite dimensions (8×8, 16×16, 32×32, 64×64)
   - Mention of retro/pixel/8-bit/16-bit aesthetic
   - Low-res source assets scaled up for display

2. **Check rendering settings.** Scan the codebase for:

   ### Canvas (HTML5 Canvas / PixiJS / Phaser)
   - `context.imageSmoothingEnabled` must be `false`
   - `canvas.style.imageRendering` must be `pixelated` or `crisp-edges`
   - Canvas dimensions should be integer multiples of the game resolution
   - If using WebGL: texture filtering must be `NEAREST`, not `LINEAR`

   ### CSS (DOM-based or hybrid)
   - `image-rendering: pixelated` on any element displaying sprites
   - `image-rendering: crisp-edges` as fallback for Firefox
   - No `transform: scale()` with fractional values on sprite elements

   ### Framework-specific
   - **Phaser 3:** `pixelArt: true` in game config, or
     `this.textures.get(key).setFilter(Phaser.Textures.FilterMode.NEAREST)`
   - **PixiJS:** `PIXI.settings.SCALE_MODE = PIXI.SCALE_MODES.NEAREST`
     or `texture.baseTexture.scaleMode = NEAREST`
   - **Three.js 2D:** `texture.magFilter = THREE.NearestFilter` and
     `texture.minFilter = THREE.NearestFilter`

3. **Check scaling math.** Verify:
   - Game renders at a low internal resolution (e.g., 320×180, 256×240)
   - Display canvas is an **integer multiple** of internal resolution
   - No fractional scaling (3.5× will cause some pixels to be larger than
     others, breaking the uniform grid)
   - Window resize handler maintains integer scaling or letterboxes

4. **Check CSS units.** If sprites are positioned with CSS:
   - Positions should use whole pixel values, not fractional
   - `transform: translate()` can cause subpixel positioning — use `left`/
     `top` with integer values instead, or round transforms

5. **Report and fix.** If issues are found, fix them directly:
   - Add `imageSmoothingEnabled = false` to canvas setup
   - Add `image-rendering: pixelated` CSS rule
   - Set texture filtering to NEAREST
   - Fix fractional scaling to nearest integer multiple

## Rules

- **Fix silently when possible.** If the fix is a one-liner
  (`imageSmoothingEnabled = false`), just add it without asking.
- **Flag scaling math issues.** If the canvas size isn't an integer
  multiple, explain why it matters and suggest the correct dimensions.
- **Don't over-engineer.** A simple `image-rendering: pixelated` CSS rule
  solves 80% of blurry pixel art issues. Start there.
- **Respect intentional smoothing.** If the user explicitly wants smooth
  scaling or is making a "pixel art inspired" HD game, don't apply this
  skill.
