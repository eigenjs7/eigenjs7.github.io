# eigenjs.com — Design System Spec

**Source of truth:** `assets/main.scss`, `_includes/head.html`, `_includes/header.html`, `_includes/footer.html`, `_layouts/*.html`, `_includes/{service,project}-card.html`, `_config.yml`.
**Last reviewed:** 2026-05-15.
**Status:** Reverse-engineered from the running stylesheet. Anything marked **Inconsistency** is what the code does today; anything marked **Open** has no source-side decision yet.

---

## 1. Brand identity

A serif-led, warm-neutral personal research site. Visual posture is academic restraint: a single accent (sienna), no UI chrome, type does the work of hierarchy. Display surfaces use Adobe Garamond Pro (Adobe Fonts), the classic Slimbach cut of Claude Garamont's roman; body and section headings use Source Serif 4 at its text optical-size cut.

- **Site title:** *Junseo Lee's Research Blog* (Adobe Garamond Pro display).
- **Tagline:** *Exploring mathematics and computation* (Source Sans 3).
- **Author / domain:** Junseo Lee, eigenjs.com.

---

## 2. Color tokens

All tokens are SCSS variables declared at the top of `assets/main.scss`. Contrast ratios computed against the `$background-color` pearl unless noted.

| Token | Hex | Role | Contrast on pearl | WCAG |
|---|---|---|---:|---|
| `$background-color` | `#faf8f5` (Pearl) | Page background | — | — |
| `$text-color` | `#4D403A` (Cacao) | Body, headings, default link | 9.39 : 1 | AAA |
| `$title-color` | `#8B5A2B` (Warm sienna) | Display titles, category pills, CTA bg | 5.51 : 1 | AA |
| `$brand-color` | `#4D403A` (Cacao) | Same as text — accent alias | 9.39 : 1 | AAA |
| `$brand-color-light` | `#A3968D` (Taupe) | Decorative only — card glyphs, italic metadata | 2.71 : 1 | **fails text** |
| `$brand-color-dark` | `#262626` (Leather) | Reserved (unused on rendered pages) | 16.5 : 1 | — |
| `$grey-color` | `#776D65` (Darker taupe) | Secondary text — taglines, post meta, footer, blockquotes | 4.76 : 1 | AA |
| `$grey-color-light` | `#DFDACF` (Khaki) | Borders, rules, separators only | 1.31 : 1 | borders only |
| `$grey-color-dark` | `#262626` (Leather) | Tag pill text | 16.5 : 1 | AAA |
| `$code-bg-color` | `#EDE5D4` (Warm parchment) | Inline `code`, `pre`, `.highlight` | 1.13 : 1 vs page | — |

**Syntax-highlighting palette (Rouge, on `#EDE5D4`):**

| Token class | Hex | Role | Contrast |
|---|---|---|---:|
| `.k, .kc, .kd…` | `#9C4221` | Keywords | 5.78 : 1 |
| `.s, .s1, .s2…` | `#356A39` | Strings | 5.12 : 1 |
| `.c, .c1…` | `#6F6358` | Comments (italic) | 4.65 : 1 |
| `.m, .mi…` | `#B5651D` | Numbers | 4.16 : 1 |
| `.nf, .nb` | `#7B4B3A` | Functions, built-ins | 6.32 : 1 |
| `.nc` | `#5D4037` | Class names | 8.59 : 1 |
| `.ss` | `#A0522D` | Ruby symbols | 4.65 : 1 |
| `.err` | `#C44536` | Errors | 4.50 : 1 |
| `.n, .o, .p…` | `$text-color` | Identifiers, punctuation | 8.31 : 1 |

**Rules of use.**
- Body, primary headings, default link foreground → `$text-color`.
- Display surfaces (site title, post title, service title, 404, about-name, year/category anchors) → `$title-color`.
- Secondary text (meta, tagline, footer links, blockquotes, "see all" intro) → `$grey-color`.
- Decorative only (card glyphs, italic card metadata) → `$brand-color-light`. **Never for body copy.**
- Borders, hairlines, separators → `$grey-color-light`.

**Inconsistency.** The card metadata (`.card-metadata`, `.service-metadata`) uses `$brand-color-light` (taupe, 2.71 : 1) for italic prose. It's italic and small, so it's almost ornamental, but it's still text and fails AA.

---

## 3. Typography

### 3.1 Font stacks

| Role | SCSS var | Stack |
|---|---|---|
| Display / title | `$title-font` | `"adobe-garamond-pro", "EB Garamond", "Garamond Premier Pro", Garamond, Georgia, serif` |
| Body & headings | `$base-font-family` / `$heading-font` | `"Source Serif 4", Georgia, "Times New Roman", serif` |
| UI / tagline | `$sans-font` | `"Source Sans 3", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif` |
| Code | `$code-font` | `"JetBrains Mono", "Fira Code", Menlo, Monaco, monospace` |

Display family is loaded from Adobe Fonts (Typekit) via a Web Project kit:
`<link rel="stylesheet" href="https://use.typekit.net/YOUR_KIT_ID.css">`. The CSS family name `adobe-garamond-pro` is Adobe's published name for the family. The rest are loaded from Google Fonts in a single request: Source Serif 4 (ital, opsz, wght), Source Sans 3 (wght 400), JetBrains Mono (wght 400, 500). Preconnect hints exist for both `use.typekit.net` and `fonts.gstatic.com`.

### 3.2 Font weights & cuts per surface

Adobe Garamond Pro is a **static family** — no `opsz` or `SOFT` axes. Available cuts in the Adobe Fonts default Web Project: Regular (400), Italic (400i), Semibold (600), Semibold Italic (600i), Bold (700), Bold Italic (700i). No 500. Earlier 500-weight surfaces (post title, service title) now use 400 — Garamond's natural stroke contrast carries presence at display size.

| Surface | Weight | Italic | Notes |
|---|---:|:---:|---|
| Site title | 400 | — | 3.75 rem, letter-spacing −0.02em |
| Post title | 400 | — | was 500 under Fraunces; remapped to 400 |
| Service title | 400 | — | was 500 under Fraunces; remapped to 400 |
| About name | 400 | — | 2.5 rem |
| 404 heading | 400 | — | 6 rem |
| Footer author name | 400 | — | 1.1 rem |

Source Serif 4 body keeps `font-variation-settings: "opsz" 14` (text optical-size cut) — that axis stays on the body family.

**Migration note.** Adobe Garamond Pro has a smaller x-height (~440 UPM at 1000) than Fraunces (~550). Display surfaces will feel ~10 % smaller at the same point size. If the site title or 404 read undersized after the swap, bump them ~5–10 % (e.g., 3.75 → 4 rem, 6 → 6.5 rem). Letter-spacing can stay tight; Garamond's roman doesn't need extra tracking until you go above ~6 rem.

### 3.3 Type scale (desktop)

Perfect-fourth modular scale (1.333×) anchored to 17 px / 1 rem body. Pure `rem`s, no fixed px on text.

| Element | Size (rem) | Approx px | Line height | Notes |
|---|---:|---:|---:|---|
| Body | 1.000 | 17 | 1.75 | `$base-line-height` |
| Small body | 0.875 | 14.9 | inherits | `$small-font-size` |
| `h1` | 2.369 | 40.3 | 1.15 | top 2.75rem / bottom 1.25rem |
| `h2` | 1.777 | 30.2 | 1.20 | top 2.5rem / bottom 1rem; in `.post-content` adds top border + 3rem/1.5rem padding |
| `h3` | 1.333 | 22.7 | 1.30 | top 1.875rem / bottom 0.75rem |
| Site title | 3.750 | 63.8 | 1.10 | letter-spacing −0.02em, balanced wrap |
| Post title | 2.000 | 34.0 | 1.20 | letter-spacing −0.01em |
| About name | 2.500 | 42.5 | 1.10 | letter-spacing −0.02em |
| Service title | 2.000 | 34.0 | 1.20 | letter-spacing −0.01em |
| 404 heading | 6.000 | 102 | 1.00 | letter-spacing −0.03em |
| Post-list `h3` (home) | 1.500 | 25.5 | 1.30 | |
| Year / category heading (archive) | 1.250 | 21.3 | — | sienna, hairline border-bottom |
| Card title | 1.333 | 22.7 | 1.30 | |
| Tagline | 0.950 | 16.2 | 1.50 | Source Sans 3 |
| Nav link | 0.900 | 15.3 | 1.00 | UPPERCASE, tracking 0.10em |
| Post meta | 0.900 | 15.3 | — | italic on post detail, normal on lists |
| Card metadata | 0.850 | 14.5 | 1.40 | italic, taupe |
| Card summary | 0.950 | 16.2 | 1.55 | |
| Service summary | 1.050 | 17.9 | 1.60 | |
| `.home-intro p` | 1.050 | 17.9 | 1.70 | hero paragraph |
| Code (inline + block) | 14 px | 14 | inherits | JetBrains Mono |
| Footer author name | 1.100 | 18.7 | — | Adobe Garamond Pro 400 |
| Footer link | 0.900 | 15.3 | — | grey |
| Footer bottom (©) | 0.750 | 12.8 | — | grey, tracking 0.05em |

### 3.4 Type scale (≤ 600 px, "on-palm")

Compressed major-third (1.25×) — content stays 1 rem.

| Element | rem | px |
|---|---:|---:|
| `h1` | 1.953 | 33.2 |
| `h2` | 1.563 | 26.6 |
| `h3` | 1.250 | 21.3 |
| Site title | 2.750 | 46.8 |
| Post title | 1.400 | 23.8 |
| Service title | 1.650 | 28.1 |
| Tagline | 0.750 | 12.8 |
| Nav link | 0.700 | 11.9 (tracking 0.04em) |
| Card image box | 56 × 56 px | — |
| Card glyph | 1.500 | 25.5 |

### 3.5 Treatment rules

- Body uses `text-rendering: optimizeLegibility` and `-webkit-font-smoothing: antialiased`.
- All headings: weight 600, letter-spacing −0.01em, color `$text-color` (display titles override to `$title-color`).
- Site title and large display surfaces use `text-wrap: balance` to avoid orphans.
- Tagline wraps with `text-wrap: balance` on mobile; `white-space: normal` to release nowrap.
- Hero paragraph and page body use `text-wrap: balance` on mobile.

---

## 4. Spacing & layout

- **Spacing unit** (`$spacing-unit`): `32 px`. Used by minima for vertical rhythm; overrides use rem-based values.
- **Content width** (`$content-width`): `750 px`. Wrapper max-width for header and service pages.
- **Listing column max** (`.posts-page`, `.archive-page`, `.about-bio`, `.home-intro`, `.page-content.centered`): `540–600 px` for measure.
- **Mobile breakpoint** (`$on-palm`): `600 px`.
- **Laptop breakpoint** (`$on-laptop`): `800 px`.
- **Extra-small breakpoint**: `360 px` — collapses two-column archive rows to single column.

**Component vertical rhythm.**

| Surface | Padding / margin |
|---|---|
| Site header | desktop `2.5rem 0 2rem`, mobile `2rem 0 1.5rem` |
| Site footer | `3rem 0 2rem`, margin-top `4rem`, top hairline |
| Post header | margin-bottom `3rem`, padding-bottom `2rem`, bottom hairline |
| About header | identical to post header |
| Service header | margin-bottom `2.5rem`, padding-bottom `1.5rem`, bottom hairline |
| Post list item | bottom margin `3rem` |
| `pre` block | `1.25em 1.5em` padding, `1.5rem 0` margin |
| Blockquote | `2em 0` margin, `0 0 0 1.5em` padding, 2px left rule |
| Service CTA | margin-top `3rem`, button padding `0.75rem 1.75rem` |

---

## 5. Components

### 5.1 Site header (`.site-header`)

Centered wrapper at `$content-width`. Composition:

1. `.header-content` — block, centered.
   - `<p class="site-title">` — Adobe Garamond Pro 400, sienna, 3.75 rem.
   - `<p class="site-tagline">` — Source Sans 3, 0.95 rem, grey.
2. `<nav class="site-nav">` — top hairline, 1 rem top padding.
   - `.page-link` — 0.9 rem, UPPERCASE, tracking 0.1em, grey by default, 2px transparent bottom border. Hover → text color. `.active` → text color + text-bottom-border. Hamburger trigger explicitly hidden.

**Inconsistency.** `_includes/header.html` uses `<p class="site-title">` (per AUDIT.md fix #2), so there is no `<h1>` in the chrome — correct per the audit. The SCSS for `.site-title` includes `display: block !important; float: none !important; width: 100%;` to defeat minima defaults.

### 5.2 Site footer (`.site-footer`)

- Top hairline (`$grey-color-light`).
- `.footer-author` — flex row, centered, 0.75rem gap.
  - `.author-name` — Adobe Garamond Pro 400, 1.1 rem, `$text-color`.
  - `.footer-separator` — middle-dot character, khaki, 0.8 rem.
  - Links — grey, 0.9 rem, no underline, hover → text color.
- `.footer-bottom` — 0.75 rem grey, tracking 0.05em, dynamic copyright.

### 5.3 Cards (`.card`)

Used by service and project listings via `_includes/service-card.html` and `_includes/project-card.html`. One `<a>` wraps the whole card for keyboard navigation.

- Layout: `display: flex; align-items: flex-start; gap: 1rem; padding: 1.25rem`.
- Border: 1px `$grey-color-light`, radius 2px, transparent background.
- Hover: border → `$title-color`; `.card-arrow` → `$title-color`.
- Visited: color reset to `$text-color` (overrides global visited grey).
- **Card image** (`.card-image`): 72 × 72 px (56 × 56 on mobile), 1px khaki border, radius 2px. Contains `<img>` or `.card-glyph`.
- **Card glyph** (`.card-glyph`): Source Serif 4 at 2 rem (1.5 rem mobile), taupe (`$brand-color-light`), normal style.
- **Card body** (`.card-body`): flex 1, `padding-right: 1.25rem` for arrow gutter, position relative.
- **Card title**: 1.333 rem Source Serif 4, weight 600, `$text-color`, tracking −0.01em.
- **Card metadata**: 0.85 rem italic, taupe.
- **Card summary**: 0.95 rem, `$text-color`, line height 1.55.
- **Card arrow** (`.card-arrow`): bottom-right absolute, 1 rem, grey → sienna on hover. Service card uses `→`; project card uses `↗︎` (text-style variation selector).

**Grid:** `.services-grid`, `.projects-grid` are CSS Grid, `grid-template-columns: 1fr`, gap 1 rem, margin 2 rem. (Single column today; could become 2-column at wider breakpoints.)

### 5.4 Post & writing surfaces

- **Post header** (`.post-header`): centered, bottom hairline.
- **Post title** (`.post-title`): Adobe Garamond Pro 400, 2 rem, letter-spacing −0.01em. Mobile 1.65 rem (overridden to 1.4 rem at `<=$on-palm` later in the file — see Inconsistency below).
- **Post meta** (`.post-meta`): 0.9 rem grey. Italic when inside post header (`font-style: italic` declared in two places — overrides apply in cascade order).
- **Post taxonomies** (`.post-taxonomies`): flex, centered, 0.5 rem gap.
  - `.category`: sienna background, pearl text, radius 3px, padding `0.25em 0.6em`, 0.7 rem, tracking 0.05em.
  - `.tag`: khaki background, leather text, otherwise identical.
- **Post content** (`.post-content`):
  - `h2` gets top hairline + padding-top 1.5 rem, margin-top 3 rem.
  - Images: radius 2px, no shadow, centered with 2 rem vertical margin.
  - Tables: wrapped in `.table-wrapper` (overflow-x: auto). Inside, 0.95 rem, 400 px minimum width, double-underline thead (2px khaki), single-underline rows (1px khaki), no zebra striping.
  - Horizontal rules render `· · ·` in khaki at 0.5em tracking.
- **Inline code**: parchment bg, 1px khaki border, radius 3px, padding `0.1em 0.3em`, vertical-align middle.
- **Block `pre`**: parchment bg, 1px khaki border, radius 4px, padding `1.25em 1.5em`, margin `1.5rem 0`, overflow-x auto. Nested `code` has transparent bg and no border.
- **Blockquote**: 2px grey left rule, 1.5em left padding, italic grey, 2em vertical margin.

**Inconsistency.** Desktop `.post-title` is set twice: 2 rem in its own block, then 1.4 rem inside the mobile media query (line ~616) — which is fine because that's `@media (max-width: $on-palm)`. But the in-block mobile override also exists (1.65 rem). The 1.4 rem rule lower in the file is the one that wins on mobile, so 1.65 rem is dead code.

### 5.5 Post list (home + archive + writings)

- **Home list** (`.home .post-list`): each `<li>` block-stacked. `h3` 1.5 rem, no underline on the link; meta is 0.9 rem grey above.
- **Archive / Writings list** (`.posts-page`, `.archive-page` → `.post-list`):
  - `li` is a 2-column CSS Grid: `85px 1fr`, gap `0.5rem 1rem`, vertical padding `0.6rem`.
  - Mobile: collapses to `75px 1fr`. Below 360 px: single column.
  - `.post-meta`: 0.8 rem grey, not italic.
  - `.post-entry`: flex column, gap 0.4 rem.
  - `.post-link`: 1.05 rem weight 500 `$text-color`; hover grey.
  - `.post-categories .category` / `.post-tags .tag`: 0.65 rem, padding `0.2em 0.5em`, sienna or khaki backgrounds (same as post taxonomies but smaller).
- **Year / category headings** (`.year-heading`, `.category-heading`): 1.25 rem Source Serif 4 weight 600, sienna, bottom hairline, top margin 2.5 rem.
- **"See all writings →"** (`.home .see-all`): 0.95 rem, sienna, khaki underline that becomes sienna on hover.

### 5.6 Service detail page

- `.service-page`: max-width `$content-width`, auto margins.
- `.service-header`: centered, bottom hairline.
  - `.service-title`: Adobe Garamond Pro 400, 2 rem sienna; 1.65 rem mobile.
  - `.service-metadata`: 0.9 rem italic taupe.
  - `.service-summary`: 1.05 rem `$text-color`, line height 1.6.
- `.service-content`: regular flow.
- `.service-cta`: top margin 3 rem, centered.
- `.service-cta-button`: inline-block, padding `0.75rem 1.75rem`, 0.95 rem, tracking 0.05em, pearl text on sienna bg, 1px sienna border, radius 2px. Hover inverts to sienna text on transparent bg with sienna border.

### 5.7 Page / about

- `_layouts/page.html` wraps content in `.page-content.centered`.
- `.page-content.centered`: max-width 540 px, auto-margined. Profile photo 110 × 110 with `border-radius: 50%`. Only `.home-intro`, `.about-header`, `.centered-text` are text-centered — body paragraphs are left-aligned (fix from audit P1-F).
- `.about-header .about-name`: Adobe Garamond Pro 400, 2.5 rem sienna, no bottom border.
- `.about-bio`: max-width 600 px. Paragraph line height 1.8. `<strong>` is weight 500 `$text-color`.

### 5.8 Home intro

- `.home-intro`: max-width 600 px, centered, margin-bottom 3 rem.
- `.profile-photo`: 110 × 110, `object-fit: cover`, full circle, margin-bottom 1 rem.
- Paragraph: 1.05 rem `$text-color`, line height 1.7 — primary copy color, not grey (fix from audit P0-B).
- Links inside: `$text-color` with grey underline that darkens on hover.

### 5.9 404

- `.page-404`: max-width 600 px, 4 rem auto vertical margin.
- `.page-404-heading`: Adobe Garamond Pro 400, 6 rem sienna, letter-spacing −0.03em.

### 5.10 Links (global)

```
color:            $text-color
text-decoration:  underline
underline-color:  $grey-color
underline-offset: 3px
visited:          $grey-color (so readers can track read state)
hover underline:  $text-color
transition:       color 0.2s, text-decoration-color 0.2s
```

Per-component overrides: site title (no underline, sienna→grey hover), site nav (no underline, custom bottom-border state), card (no underline, border-color hover), CTA (filled, inverts on hover), footer (no underline, grey→text hover), post-link (no underline, text→grey hover), see-all (custom khaki→sienna bottom border).

---

## 6. Iconography & ornaments

- **Favicon:** `/assets/images/favicon.svg` (referenced as both `rel="icon"` and `rel="apple-touch-icon"`).
- **Profile image:** `/assets/images/profile.jpg`, rendered at 110 × 110.
- **Card glyphs:** opt-in per item via `glyph:` front-matter (default `·`). Rendered in Source Serif at 2 rem, taupe.
- **Card arrows:** `→` for internal (services), `↗︎` for external (projects).
- **HR ornament:** `· · ·` with 0.5em tracking, khaki.
- **Footer separators:** middle-dot, khaki.

---

## 7. Responsive system

Two breakpoints in active use plus an extra-small fallback.

| Token | Value | Effect |
|---|---|---|
| `$on-palm` | 600 px | Header reflow, type scale compresses, nav becomes flex `space-between` with 0.7 rem uppercase links |
| `$on-laptop` | 800 px | Defined but minima-owned (used inside the gem for the wrapper inflection) |
| 360 px (hard-coded) | 360 px | Archive grid collapses to single column |

Mobile nav uses `flex: nowrap; justify-content: space-between` so all five nav items fit on one line at 360 px without truncating. Touch target is `padding: 0.75rem 0` → ~44 px tall.

---

## 8. Accessibility checklist

- Body, headings, default link foreground all meet **AAA** (9.39 : 1) on pearl.
- Sienna display titles, sienna category pills, CTA button all meet **AA** (5.51 : 1).
- `$grey-color` darkened to `#776D65` so all secondary text now passes AA (was `#A3968D`, 2.71 : 1).
- Link underlines now use `$grey-color`, not `$grey-color-light` — visible by default.
- Code background `#EDE5D4` is now 1.13 : 1 against pearl plus a 1px border — visually distinct.
- Code syntax colors: keywords, strings, comments, functions, classes, errors all ≥ 4.5 : 1 on parchment.
- Site chrome has a single `<h1>` per page (post/page content, not the header).
- External links open in `_blank` with `rel="noopener noreferrer"` via inline JS in head.
- Card images carry `alt=""` and the wrapping `.card-image` has `aria-hidden="true"`; card arrows are `aria-hidden`.
- Mobile nav touch target ≈ 44 px tall.
- Tables get auto-wrapped in `.table-wrapper` with `-webkit-overflow-scrolling: touch` so they remain scrollable on small screens without breaking layout.

**Known gaps.** Italic card metadata uses taupe (`$brand-color-light`, 2.71 : 1) — fails AA for text. Move it to `$grey-color` or accept that it's ornamental and small. No skip-to-content link. No `prefers-color-scheme: dark` palette.

---

## 9. Page templates

| Template | Layout | Distinguishing classes | Notes |
|---|---|---|---|
| Home | `home` → `default` | `.home`, `.home-intro`, `.post-list`, `.see-all` | Hero bio + recent-5 post list |
| Post | `post` → `default` | `.post`, `.post-header`, `.post-title`, `.post-taxonomies`, `.post-content` | `h-entry` microformat, schema.org BlogPosting |
| Writings index | `posts` → `default` | `.posts-page`, `.category-heading` | Grouped by category |
| Archive | `archive` → `default` | `.archive-page`, `.year-heading` | Grouped by year |
| Service detail | `service` → `default` | `.service-page`, `.service-header`, `.service-cta-button` | CTA wired to `register_url` front-matter |
| Services index | `page` → `default` | `.services-grid`, `.card` | Card list |
| Projects index | `page` → `default` | `.projects-grid`, `.card` | Card list, external links |
| About / generic | `page` → `default` | `.page-content.centered`, `.about-header`, `.about-bio` | Long-form left-aligned, intro centered |
| 404 | `page` → `default` | `.page-404`, `.centered-text` | 6 rem display heading |

Front-matter fields used across templates:

- **Posts**: `title`, `date`, `categories`, `tags`, `math` (gates MathJax), `image` (for og).
- **Services** (`_services/`): `title`, `metadata`, `summary`, `glyph`, `image`, `register_url`.
- **Projects** (`_projects/`): `title`, `metadata`, `summary`, `glyph`, `image`, `external_url`.

---

## 10. Behavior & build

- **Theme base:** `minima` gem. `assets/main.scss` declares variables before `@import "minima"` and then layers a complete override stylesheet.
- **Font hosting:** Adobe Garamond Pro is delivered from Adobe Fonts via a Web Project kit (`use.typekit.net/YOUR_KIT_ID.css`). The kit ID is provisioned in the Adobe Fonts dashboard and pasted into `_includes/head.html`. The remaining families are Google Fonts.
- **Markdown:** kramdown, GFM input, MathJax math engine, Rouge syntax highlighter.
- **Plugins:** `jekyll-feed`, `jekyll-seo-tag`, `jekyll-sitemap`.
- **MathJax:** loaded conditionally on `page.math == true` or `site.math_everywhere`. `inlineMath: $…$, \(…\)`. `displayMath: $$…$$, \[…\]`. Skips `pre`, `code`, `script`, `noscript`, `style`, `textarea`.
- **External links:** post-DOM JS in head opens off-host `http(s)` links in `_blank` with `rel="noopener noreferrer"`.
- **Table wrapping:** same DOM-ready pass wraps any unwrapped `.post-content table` in `.table-wrapper`.
- **Fonts:** single Google Fonts `<link>` with `display=swap`, preconnect hints.
- **SEO:** `jekyll-seo-tag` reads `site.title`, `site.description`, `site.image` (currently `/assets/images/profile.jpg`), and per-page `image:`. Twitter card is `summary` — upgrade to `summary_large_image` when a real 1200 × 630 OG image lands.

---

## 11. Component inventory (quick reference)

```
.site-header           .site-footer           .site-nav
  .site-title            .footer-author         .page-link[.active]
  .site-tagline          .author-name
                         .footer-separator
                         .footer-bottom

.home                  .posts-page            .archive-page
  .home-intro            .category-heading      .year-heading
    .profile-photo       .post-list
  .post-list-heading       li > .post-meta, .post-entry > .post-link, .post-tags
  .post-list             .post-categories .category
    li > h3 > a          .post-tags .tag
  .see-all > a

.post                  .service-page          .page-404
  .post-header           .service-header        .page-404-heading
    .post-title          .service-title
    .post-meta           .service-metadata
    .post-taxonomies     .service-summary
      .category, .tag  .service-content
  .post-content          .service-cta
    h2, img, table         .service-cta-button
    blockquote, hr
    code, pre, .highlight

.services-grid         .projects-grid         .about-bio
  .card                  .card                  .about-header
    .card-image            .card-image            .about-name
      img | .card-glyph      img | .card-glyph    .about-meta
    .card-body             .card-body
      .card-title            .card-title
      .card-metadata         .card-metadata
      .card-summary          .card-summary
      .card-arrow            .card-arrow

.page-content.centered
  .home-intro | .about-header | .centered-text
  .profile-photo
```

---

## 12. Open questions / decisions to make

1. **Card metadata color.** Italic taupe fails AA. Either accept ornamental-only treatment or promote to `$grey-color`.
2. **Visited link policy.** Global `a:visited → $grey-color` is set; the card and CTA override back to text/pearl. Confirm we *want* lists of post titles (rendered as `.post-link`, not `.card`) to show visited state — they currently inherit grey, which is the desired behavior.
3. **Dark mode.** Not implemented; serif-heavy reading sites commonly carry one. A ~40-line `prefers-color-scheme: dark` block swapping `$background-color`, `$text-color`, `$code-bg-color`, and tightening `$grey-color` would do it.
4. **OG image.** Currently the 110 × 110 profile JPEG. A dedicated 1200 × 630 share image at `/assets/images/og.png` + `twitter:card: summary_large_image` would render proper cards.
5. **Card grid responsiveness.** Single column today. At wider viewports we could move to `grid-template-columns: repeat(auto-fill, minmax(320px, 1fr))` without other changes.
6. **Project card external indicator.** The arrow is `↗︎`; consider also a `target="_blank"` icon for screen-reader-friendly affordance (currently relies on the arrow + `target="_blank"`).
7. **Reading time / word count on `.post-header`.** One-line Liquid: `{{ content | number_of_words | divided_by: 200 }} min`.
8. **Code copy button.** Not implemented; a 30-line vanilla JS pass on `pre > code` would deliver one.
9. **Section dividers on listings.** The `· · ·` HR is only in `.post-content`. Listing pages currently rely on padding alone for rhythm.
