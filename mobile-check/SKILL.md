---
name: mobile-check
description: "Verifies mobile responsiveness after CSS or layout changes"
version: "1.0.0"
source: custom
---

# Mobile Responsiveness Check

After any CSS or layout change, verify the result looks correct on mobile
viewports before reporting the task as done.

## Workflow

1. **Determine the affected page(s).** Look at which files were modified and
   identify the route(s) that would be impacted.

2. **Check if the dev server is running.** If working on the portfolio or any
   Next.js project, verify `localhost:3000` (or the project's dev port) is
   accessible.

3. **Test at iPhone 14 viewport (390×844).** Using Playwright or the browser
   tool:
   - Resize to 390×844
   - Navigate to each affected page
   - Take a screenshot
   - Check for: content overflow, text truncation, touch target sizes (min
     44px), horizontal scroll where there shouldn't be, images breaking layout,
     elements overlapping

4. **Test at iPad viewport (768×1024)** if the change involves grid layouts
   or breakpoint-specific code (sm:, md:, lg: classes).

5. **Report findings.** Show the user the mobile screenshot and flag any issues:
   - Content overflowing the viewport horizontally
   - Text too small to read (under 12px)
   - Touch targets too small (under 44×44px)
   - Layout shifts or overlapping elements
   - Elements hidden that shouldn't be, or visible that should be hidden

6. **Fix issues immediately** if they're clearly bugs from the change just made.
   Don't ask — just fix obvious mobile regressions and show the before/after.

## Edge Cases

- If Playwright isn't available or the browser tool fails, fall back to
  reading the CSS/Tailwind classes and checking for missing responsive
  prefixes (sm:, md:, lg:) on layout-critical properties.
- If the dev server isn't running, tell the user and suggest they start it.
- If the change is purely a color or opacity tweak with no layout impact,
  skip the full viewport test — just confirm the change applies correctly.

## What to Watch For (Common Mobile Issues)

- `overflow-hidden` missing on containers with absolute-positioned children
- Fixed widths (`w-[500px]`) without responsive alternatives
- Font sizes set in px without mobile scaling
- `gap` values too large for mobile (gap-8+ often needs gap-4 on mobile)
- Missing `px-4` or equivalent horizontal padding on mobile
- `whitespace-nowrap` causing horizontal overflow
- Images without `max-w-full` or proper `sizes` attribute
