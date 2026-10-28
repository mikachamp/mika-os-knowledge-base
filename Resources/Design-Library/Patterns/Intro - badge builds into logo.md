---
tags: [design-library, pattern, motion, intro, logo, own-work]
effort: M
origin: own-work
---
# Intro - badge builds into logo
> **Own work.** A page intro of about 3.4 s: the logo's simplest symbol appears alone and draws itself, then travels to its true place while the wordmark builds out of it letter by letter. The finished logo already sits where the hero logo is, so the page fades in around it with no jump.

## Structure
```html
<head>
  <style>/* overlay + early cover + reduced-motion guard */</style>
  <script>/* decide early whether to play (sessionStorage, reduced motion, #hash, already scrolled) so the page never flashes */</script>
</head>
<body>
  ... existing page, hero <img class="hero-logo"> untouched ...
  <template id="intro-tpl">
    <svg viewBox="0 0 240 120">          <!-- generic "MW" monogram, split into parts; use your own logo here -->
      <g class="badge"><circle class="ring" cx="60" cy="60" r="44" fill="none" stroke="currentColor" stroke-width="3"/><circle class="fill" cx="60" cy="60" r="38"/></g>
      <g class="word-1"><path class="letter" d="M34 82V38l14 26 14-26v44"/></g>   <!-- M -->
      <g class="word-2"><path class="letter" d="M120 38l12 44 12-30 12 30 12-44"/></g>   <!-- W -->
      <g class="tagline"><path class="line" d="M120 96h72"/></g>
    </svg>
  </template>
  <script>/* timeline */</script>
</body>
```
- Use the **real vector logo** (letters as paths). Split any path shared between letters so each letter moves on its own.
- The overlay SVG sits over the hero logo's box and is **re-measured every frame**, so a late font load cannot knock it out of alignment.

## Behaviour (timeline)
| Time (s) | What happens | Easing |
|---|---|---|
| 0.00-0.55 | Badge fades and scales in at screen centre, slightly oversized, soft glow | expo-out `cubic-bezier(.16,1,.3,1)` |
| 0.20-0.85 | Symbol draws itself (stroke-dash) | `cubic-bezier(.33,1,.68,1)` |
| 0.55-1.00 | Ring grows out from behind the badge | expo-out |
| 0.85-1.85 | Badge travels to its true place and shrinks to real size; glow fades | in-out `cubic-bezier(.65,0,.35,1)` |
| 1.35-2.28 | Nearest word wipes out of the badge one letter at a time | expo-out (clip wipe) |
| 1.70-2.66 | Second word rises from behind its baseline, outward from the symbol | expo-out (masked rise) |
| 2.20-3.00 | Small spaced line fades in and tracks out to its spacing | expo-out |
| 2.85-3.35 | Backdrop fades; the overlay is removed and the real `<img>` shows in the same frame | in-out |

- **Library:** none. Plain CSS and JS; only SVG transform, opacity, dash and clip attributes animate.
- **Skip:** click, tap, any key, wheel, touch-move or scroll jumps to the end at once.
- **Once per session:** `sessionStorage` inside try/catch. Skipped when the page opens on a `#link` or is already scrolled.
- **Reduced motion:** no intro at all.
- **Failsafe:** a 6 s timeout removes the early cover if the script never runs.
- **Accessibility:** the overlay is `aria-hidden` and purely visual; the real heading stays in the accessibility tree.
- **Seamless hand-over trick:** if the hero has a glow or tint over the logo area, add a matching tint masked to the logo shapes and fade it in with the backdrop, otherwise the swap shows a small colour step.

## When to use it
- **Use it for:** brands whose logo has a clear standalone symbol (badge, monogram, icon), on the first visit to a one-page site.
- **Avoid it for:** shops where people come to buy quickly (keep it under 2 s or skip it), pages people reload often, logos with no separable symbol.

## Effort to rebuild
**M.** About 190 lines in one page. Most of the work is splitting the logo SVG into parts and nailing the hand-over.
