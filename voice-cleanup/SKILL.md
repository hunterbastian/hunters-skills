---
name: voice-cleanup
description: "Silently parses voice-dictated input from Wispr"
version: "1.0.0"
source: custom
---

# Voice Cleanup

The user uses Wispr for voice-to-text. Sometimes their input arrives as a
stream-of-consciousness dictation that needs parsing, not correction.

## Rules

1. **Never comment on the input quality.** Do not say "It looks like you
   dictated this" or "Let me clean that up." Just understand what they meant
   and respond normally.

2. **Parse multiple instructions.** Voice input often packs several requests
   into one run-on message. Break them apart and address each one.

   Example input: "okay so lets change the header to be a little bigger and
   also the footer needs to be darker and can you check if the mobile menu
   is working i think its broken"

   Parse as three separate tasks:
   - Make the header bigger
   - Make the footer darker
   - Check if mobile menu is working (potential bug)

3. **Resolve homophones.** Use context to determine the correct word:
   - "their" vs "there" vs "they're"
   - "its" vs "it's"
   - "to" vs "too" vs "two"
   - "then" vs "than"
   - "effect" vs "affect"
   - "are" vs "our"

4. **Interpret phonetic misspellings.** Voice-to-text sometimes produces
   near-misses. Use context:
   - "tail wind" → Tailwind
   - "next js" or "next yes" → Next.js
   - "framer motion" might come through as "frame or motion"
   - "react" might come through as "re-act"
   - Technical terms may be split into multiple words

5. **Ignore filler words.** Strip out: "okay so", "basically", "like",
   "um", "uh", "you know", "i mean", "kind of", "sort of", "i guess",
   "right", "yeah so" — these carry no instruction.

6. **Preserve the user's intent exactly.** Do not add your own
   interpretation of what they "probably also want." Parse what they said,
   execute what they asked. Nothing more.

## Edge Cases

- If the voice input is genuinely ambiguous (could mean two different things),
  ask ONE clarifying question. Do not ask multiple.
- If the input contains a technical term you can't resolve from context,
  ask what they meant rather than guessing wrong.
- If the input is just a greeting or casual chat ("hey whats up how are you"),
  respond conversationally. Don't try to parse it as a task.
