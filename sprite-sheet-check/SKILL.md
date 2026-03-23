---
name: sprite-sheet-check
description: "Validates sprite sheet dimensions when added to 2D games"
version: "1.0.0"
source: custom
---

# Sprite Sheet Check

When a sprite sheet or tile map is added to a 2D game project, validate that
it's set up correctly before the user discovers broken animations at runtime.

## Workflow

1. **Identify the sprite sheet.** Find the image file and any code that
   references it. Look for:
   - Frame width/height definitions
   - Frame count or column/row count
   - Animation definitions (idle, walk, attack, etc.)

2. **Validate dimensions.** Check that:
   - The image width is evenly divisible by the frame width
   - The image height is evenly divisible by the frame height
   - `(width / frameWidth) * (height / frameHeight) >= declared frame count`
   - No off-by-one: if the sheet has padding/gutters between frames, verify
     the code accounts for them

3. **Check animation frame ranges.** For each animation definition:
   - Start frame + frame count doesn't exceed total frames
   - No overlapping frame ranges (unless intentional for shared frames)
   - Idle animation exists (most common missing animation)

4. **Verify loading code.** Check that:
   - The image path is correct and the file exists at that path
   - The sprite sheet is loaded before being used (async loading handled)
   - Any atlas JSON/XML metadata file matches the image

5. **Report findings.** If everything checks out, say nothing — don't
   interrupt the workflow. If there are issues:
   > "Sprite sheet issue: [image.png] is 256×128 but frame size is set to
   > 48×48 — that's 5.33 columns, so the rightmost frames will be clipped.
   > Should the frame size be 32×32 (8×4 = 32 frames) or 64×64 (4×2 = 8
   > frames)?"

## Common Issues

- **Non-power-of-two dimensions:** Some renderers require POT textures.
  Flag if the target renderer needs it (WebGL often does).
- **Padding/margin confusion:** Texture packers add 1-2px padding between
  frames. If the code doesn't account for this, frames will have slivers
  of adjacent frames bleeding in.
- **Row-major vs column-major:** Some engines read left-to-right then
  top-to-bottom, others read top-to-bottom then left-to-right. Verify
  the code matches the sheet layout.
- **Alpha channel:** PNG sprite sheets should have transparency. If the
  user is using JPG, flag that they'll lose transparency.

## Rules

- **Silent on success.** Only speak up if there's an actual problem.
- **Be specific about numbers.** "Frame 12 is out of bounds" is useful.
  "There might be an issue" is not.
- **Don't block.** Report the issue and suggest a fix, don't refuse to
  proceed.
