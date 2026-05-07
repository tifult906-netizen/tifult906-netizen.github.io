# Shemesh Landing Page

> This file is auto-loaded by Claude (in Claude Code or referenced manually in chat) at the start of every session. Keep it accurate, keep it short, keep it ground truth.

## Project Context

- **Type:** Hebrew/RTL landing page
- **Deployment:** GitHub Pages — https://tifult906-netizen.github.io
- **Repo:** https://github.com/tifult906-netizen/tifult906-netizen.github.io
- **Stack:** Single `index.html` (inline CSS + JS), no build step, no framework
- **Logo:** `לוגו_שמש.jpg` in repo root
- **Form integration:** Submits to a Make.com webhook → Surense CRM (createLead API)

## Critical Constraints

These must not break or the site stops working.

- **All UI text is Hebrew.** Always test RTL rendering after any layout change.
- **Phone format sent to Surense:** `0500000000` (10 digits, **no dash**). Form-side regex accepts the dash for user input convenience, but `replace(/\D/g, '')` strips non-digits before the webhook POST.
- **ID validation:** 9 digits with valid Israeli ID checksum. Don't skip the checksum.
- **Webhook expects flat form-encoded fields:** `full_name`, `id_number`, `birth_date`, `phone`, `language`, `utm_source` — NOT a nested `value` blob. Make.com parser is shallow. `consent` is enforced client-side only and is NOT sent.
- **Single static file.** No build step. No bundlers. No npm. Must remain deployable as `index.html` to GitHub Pages.
- **Logo file path** must stay relative (`./לוגו_שמש.jpg`). Don't hardcode absolute URLs.

## Anti-Slop Frontend Rules

Pasted from the Anthropic Frontend Aesthetics Cookbook. These keep output from converging on generic AI-style design.

```
<frontend_aesthetics>
When designing or modifying frontend interfaces, actively avoid AI-generated aesthetic defaults:

- NEVER use Inter, Roboto, Arial, or generic system fonts. Pick distinctive
  typefaces with character.
- NEVER use purple gradients on white backgrounds. Avoid the cliché altogether.
- NEVER produce predictable layouts (centered hero, three-column features,
  testimonial slab, footer). Break the grid intentionally.
- NEVER use evenly-distributed pastel palettes. Commit to one or two dominant
  colors with sharp accents.

Instead:
- Specify exact dimensions, spacing scales, and grid structures.
- Reference named aesthetic traditions (editorial print, brutalist web,
  Swiss design, art deco, organic naturalism, etc.) — not vague adjectives.
- Pick a single conceptual direction and execute it with precision.
- Use motion sparingly but with intent — one orchestrated load animation
  beats scattered micro-interactions.

Match implementation complexity to the aesthetic vision. Maximalist designs
need elaborate code. Minimalist designs need restraint and meticulous
spacing/typography.
</frontend_aesthetics>
```

## Hebrew Typography Notes

- Hebrew fonts to consider: **Heebo, Assistant, Rubik, Frank Ruhl Libre, Suez One**. Pick one for display, one for body, commit.
- DO NOT pick a Latin font with Hebrew fallback. Hebrew letterforms have their own logic and need a Hebrew-first design.
- Numerals in forms: tabular alignment so the form doesn't shift as users type.
- LTR-inside-RTL contexts: phone numbers, IDs, dates. Wrap these in `<bdi>` or `dir="ltr"` containers to prevent visual scrambling.
- Test on Safari iOS specifically — RTL bugs hide there.

## What Not to Do

- Don't change form field names without updating the Make.com webhook parser. They are coupled.
- Don't add a build step. Must remain deployable as a single static file.
- Don't generate placeholder Lorem Ipsum in Hebrew copy — translate properly or leave English placeholder marked `[TODO: translate]`.
- Don't use Inter, Roboto, Arial, or default system fonts.
- Don't use purple-on-white gradient hero sections (canonical AI slop).
- Don't auto-commit to git or auto-push to GitHub. Avihay reviews and pushes manually.
- Don't read or write files outside this project folder.
- Don't store secrets, API keys, or credentials in any file in this repo.

## Workflow Expectations

- **Brainstorm before code.** For any change beyond a one-line fix, talk through approach first. No "just start writing."
- **Wireframe before mockup.** Structural changes get an ASCII wireframe first.
- **Mockups before HTML.** Visual changes get inline Visualizer mockups for review before HTML edits.
- **Anti-slop review before "done."** Every phase passes the cookbook checklist before being marked complete.
- **One feature at a time.** Don't bundle unrelated changes into a single edit pass.

## See Also

- `DESIGN.md` — visual decisions and design system
- `chat-workflow.md` — the five-phase workflow when working in claude.ai chat
- `claude-code-setup.md` — local setup reference
