---
name: bookmark
description: "Auto-saves URLs to Obsidian when user shares a link"
user_invocable: true
version: "1.0.0"
source: custom
---

# Bookmark

When the user sends a URL, save it to `hunter obsidian/inbox/saved-links.md` before doing anything else.

## Steps

1. **Read `saved-links.md`** to see current categories and format.
2. **Categorize the link.** Match to an existing section header:
   - `## youtube` — YouTube links
   - `## x` — X/Twitter links
   - `## articles` — blog posts, Reddit, newsletters
   - `## tools` — design tools, dev tools, utilities, color pickers, etc.
   - `## learn` — courses, tutorials, educational platforms
   - `## shopping` — product pages, stores
   - `## inspiration` — design portfolios, Dribbble, Behance, galleries
   - If none fit, create a new `## section` with a lowercase name.
3. **Add the link** at the top of the matching section (newest first) with a short description:
   ```
   - https://example.com — short description of what it is
   ```
4. **Confirm** with a one-line message: what was saved and where.
5. **Then** optionally discuss, fetch, or answer questions about the link.

## Rules

- Always bookmark FIRST. Discussion is secondary.
- Keep descriptions short (3-8 words).
- If the user provides context ("this is for Studio Alpine"), include it in the description.
- Don't duplicate links that already exist in the file.
- Newest links go at the top of each section.

## File Location

`~/Documents/Obsidian Vault/hunter obsidian/inbox/saved-links.md`
