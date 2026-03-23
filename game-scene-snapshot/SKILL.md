---
name: game-scene-snapshot
description: "Dumps Three.js scene graph when visual issues reported"
version: "1.0.0"
source: custom
---

# Game Scene Snapshot

When the user reports a visual issue in a Three.js scene and you cannot
screenshot it (Playwright doesn't support WebGL), diagnose the problem by
reading the scene code instead.

## Workflow

1. **Do NOT attempt a Playwright screenshot.** WebGL scenes render as blank
   or broken in the Playwright browser. Do not waste time trying.

2. **Identify the scene file(s).** Find where the Three.js scene is set up:
   - Look for `new THREE.Scene()`, `scene.add()`, or scene composition code
   - Find the camera setup (`PerspectiveCamera`, `OrthographicCamera`)
   - Find the renderer setup and any post-processing

3. **Build a scene inventory.** Read the code and compile:

   ```
   SCENE SNAPSHOT
   ==============
   Camera: [type] at [position] looking at [target], FOV [value]
   Renderer: [size], [pixel ratio], [tone mapping]
   Lights:
     - [type] at [position], color [hex], intensity [value]
     - ...
   Objects:
     - [name/description] at [position], scale [value], material [type/color]
     - ...
   Post-processing: [effects if any]
   Known issues: [anything obviously wrong from reading the code]
   ```

4. **Diagnose from the inventory.** Common issues:
   - **Objects invisible:** Check if they're at position (0,0,0) inside another
     object, or if the material is transparent/wrongly configured
   - **All black:** Missing lights, or lights at intensity 0, or wrong
     tone mapping
   - **Wrong colors:** Check material color values, light colors, and whether
     textures are loading (look for texture load callbacks/errors)
   - **Camera seeing nothing:** Camera position too far, looking the wrong
     direction, or near/far clipping planes wrong
   - **Z-fighting/flickering:** Objects at the same position, or near plane
     too small
   - **Objects too big/small:** Scale mismatches between objects

5. **Report findings.** Present the scene snapshot and your diagnosis.
   Suggest specific code changes to fix the issue.

6. **After fixing:** Run `tsc --noEmit` or the project's build command to
   verify no type errors. Then tell the user to check in their browser.

## Rules

- NEVER try to screenshot a Three.js/WebGL scene with Playwright.
- NEVER say "I can't verify this visually." Instead, read the code and
  diagnose from the scene graph.
- Always suggest the user verify in their actual browser after your fix.
- If the scene is complex, focus on the area the user mentioned rather
  than inventorying the entire scene.
