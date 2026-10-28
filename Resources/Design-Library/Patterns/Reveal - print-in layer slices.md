---
tags: [design-library, pattern, motion, reveal, own-work]
effort: S
origin: own-work
---
# Reveal - print-in layer slices
> **Own work.** An image is revealed bottom-up in stepped horizontal slices with a glowing "nozzle" line at the edge, as if it were being 3D-printed as you scroll to it. Text and prices are always visible; only the art prints.

## Structure
```html
<article class="card">
  <div class="art print-in" style="--delay:.12s"><img ...><i class="nozzle-line"></i></div>
  <h3>Product</h3><p class="price">Always visible</p>
</article>
```
```css
.print-in { clip-path: inset(100% 0 0 0); }
.print-in.in { animation: printUp 1.1s steps(14, end) var(--delay) forwards; }   /* 14 stepped slices */
@keyframes printUp { to { clip-path: inset(0 0 0 0); } }
.nozzle-line { /* 2px glow line that rides the clip edge, then fades */ }
```

## Behaviour
- **Trigger:** IntersectionObserver, staggered by column (`--delay`).
- **Motion:** only `clip-path` and `opacity` animate. `steps()` gives the layer look: 7 slices for small letters, 14 for images.
- **Reduced motion:** everything is shown in its final state.

## When to use it
- **Use it for:** product grids, logos and wordmarks, galleries on maker or fabrication themed sites.
- **Avoid it for:** hiding text or prices, or many cards printing at once (stagger, or limit to the first row).

## Effort to rebuild
**S.**
