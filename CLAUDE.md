# MixLab — mixlab.sankhacooray.com

Single-file HTML5 app (`index.html`), GitHub Pages: pushing `main` deploys. Same design system as Coffee Lab
(`../coffeelab-sankhacooray-com/CLAUDE.md`): `ui-ux-style` rules 33/36/37 — nothing scrolls, lists page, details in tabs.

## Layout
- App bar: logo · ingredient search (opens over the bar on phones; `/` focuses it) · theme switch · CTA "Mix a sample drink".
- Left pane (`#drinkside`): drink-type pills · answer ("Your smoothie · 296 kcal · 383 g") · glass (SVG, `renderGlass`) ·
  bottom tabs **Mix | Nutrition | Compounds | Health | Taste** (`.dv` views, one at a time; Health shows a caution count badge).
- Containers (`CONTAINERS`, `renderGlass`): tumbler + straw (Smoothie), highball (Juice), soda-fountain glass (Shake),
  glass mug (Coffee), ceramic bowl with toppings (Yogurt Bowl); fill = grams / container capacity.
  Narrow (<900px): grid with the glass in the right column (`--glasscol`), tabs spanning the full width.
- Right pane: category pills + ingredient tiles (emoji, kcal · serving, + to add), paged with dots; ↑/↓ or wheel turns pages.

## Data (don't change without asking)
`INGREDIENTS` (macros, micros, compounds, benefits, warnings), `INTERACTIONS`, `DISEASE_CHECKS`, `SAMPLE_MIXES`.
`analyze()` derives totals, interactions and condition notes; the UI only renders them.
Taste: `TASTE_AXES` = five basic tastes + pungent, astringent, body; `TASTE` scores per serving (estimates);
`tasteProfile()` dilutes by weight and applies masking (sweet/salt → bitter, sweet → sour, dairy/fat → astringent, pungent).
Sources are listed in the Taste tab's ⓘ.

## No-scroll machinery
`fitPages()` pages the tiles (`data-stretch` grows rows to fill); `fitDrinkView()` pages the current tab's `.plist`
(Health first hides the benefit chips). `#scn-footer` reserves the shared footer's height (56px, 52px under 560px).

## Deploy
Bump `CACHE` in `sw.js` (`mixlab-vN`), push `main`, curl the live page with `?nc=$RANDOM` until the new markup shows.
Fit-check in headless Chrome at 1352×690, 1280×640, 1920×1080, 820×1180, 600×1000, 390×844, 375×667, 360×740.

## Lighthouse
<!-- lighthouse-optimized: 2026-10-31 -->
Baseline (mobile): performance 74, accessibility 91, best-practices 100, SEO 100.
Fixed: Google Fonts non-blocking with preconnects (dropped unused DM Sans 300/500, serif italic, Mono 500); `--text-muted`/`--text-dim` raised to ≥4.5:1 in both themes; tile and list pager dots now have 24px tap targets (same 9px/8px dots).
Left: inline JS isn't minified (single-file app, no build step); gtag/shared-script findings are handled centrally. GitHub Pages can't set cache headers.
