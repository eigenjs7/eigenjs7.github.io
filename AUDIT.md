# eigenjs.com — Full Audit

**Scope:** code quality + design & visuals, repo + rendered `_site`
**Date:** 2026-04-17
**Auditor notes:** the live domain (`www.eigenjs.com` / `eigenjs.com`) was blocked by the network egress proxy in this environment, so I used the locally built `_site/` directory as the rendered reference. The `_site` build dates from 2026‑03‑02 and matches the current source, so findings should hold against the live site — re-verify anything marked **LIVE** after we clear these up.

---

## TL;DR — priorities

1. **No posts are published.** `_posts/2026-02-28.md` is not named in Jekyll's required `YYYY-MM-DD-title.md` format, and `_posts/Numerical Analysis/1. Numerical Analysis.md` has no front matter and also fails the filename pattern. Result: `/posts/` and `/archive/` both render the literal string "No writings yet." and the feed is empty. This is the single biggest issue — everything else is polish.
2. **~300 lines of dead SCSS and a dead `custom-head.html`** encode an earlier design iteration (Merriweather/Playfair, DM Serif Display/Fraunces). They are never imported and only create confusion.
3. **Two `<h1>` per post page.** Site title and post title are both `<h1>`. Accessibility and SEO regression.
4. **A core accent color fails WCAG AA** at 2.71:1. It's used for taglines, post meta, the home bio, footer links, and year/category headings — almost all your secondary text.
5. **Homepage has no on‑ramp.** The landing page only shows a 110 px avatar and one sentence. There's no recent‑posts list or "start here," so a first visitor has nothing to click.

If you only fix three things, fix #1, #3, and #4.

---

## Part 1 — Code quality

### P0 — Must fix

#### 1. Posts aren't being published

- `_posts/2026-02-28.md` — filename is `YYYY-MM-DD.md` with no title slug. Jekyll requires `YYYY-MM-DD-title.md`. It is silently ignored. Front matter also uses a placeholder `title: Your Post Title` and `permalink: /custom-url/`.
- `_posts/Numerical Analysis/1. Numerical Analysis.md` — no front matter, wrong filename pattern, space in folder name. Won't be picked up.
- `_posts/Data Analysis/`, `_posts/Fluent Python/`, `_posts/Web Development/` — empty.
- `_posts/template/{{date}}-title.md` — template file literally named with Liquid-looking braces; also wrong filename and has garbled `date:` YAML (`"{ date: YYYY-MM-DD }":`). Keep the template, but move it *outside* `_posts/` (e.g. into `_drafts/` or a top-level `_templates/` excluded in `_config.yml`) so Jekyll doesn't try to process it.
- `_posts/.obsidian/` — your Obsidian vault config is checked into the repo. Doesn't ship (Jekyll excludes dot-folders), but clutter.

**Effect on the rendered site:** `/posts/` and `/archive/` print "No writings yet." (verified in `_site/posts/index.html:84`, `_site/archive/index.html:84`). `feed.xml` is also empty of items.

**Fix:** rename the real post to `2026-02-28-numerical-analysis-intro.md` (or similar), give it complete front matter, and either (a) consolidate content into it or (b) move the Numerical Analysis note under a proper post filename and add front matter. Move the template out of `_posts/`. Add `.obsidian/` to `.gitignore`.

#### 2. Two `<h1>` per post page

- `_includes/header.html:4` — `<h1 class="site-title">`
- `_layouts/post.html:7` — `<h1 class="post-title p-name">`

On any post page both render; screen readers and SEO crawlers see competing top-level headings. The h-entry/schema.org structured data also gets muddy.

**Fix:** demote the site title to `<p class="site-title">` (or `<span>`), and keep `<h1>` for page/post content. Most Jekyll themes do exactly this — including current versions of minima.

### P1 — Should fix

#### 3. Dead SCSS files

`_sass/minima/custom-styles.scss` (~240 lines) and `_sass/minima/custom-variables.scss` define a completely different visual system (Merriweather body, Playfair Display headings, warm‑academic palette with `$content-background`, `$border-color`, `$link-color`, `$heading-font-family`, etc.). Nothing imports them. Your real stylesheet is `assets/main.scss`, which `@import "minima"` from the gem and then overrides. Having both makes future edits a minefield — it's easy to edit the dead file and wonder why nothing changed.

**Fix:** `git rm -r _sass/minima/`. Keep everything in `assets/main.scss` (or, cleaner, break `main.scss` into `_sass/_variables.scss`, `_sass/_typography.scss`, etc. imported from `main.scss`).

#### 4. Dead `_includes/custom-head.html`

Requests a second set of Google Fonts (DM Serif Display, Fraunces) plus a duplicate MathJax config. `_includes/head.html` does not include it. Minima's gem-level `head.html` does `{% include custom-head.html %}`, but you've overridden `head.html` locally so that fallback never fires.

**Fix:** delete `_includes/custom-head.html`. Keep the fonts that you actually use in `head.html` (IBM Plex Serif/Sans, Cormorant Garamond, Source Serif 4, JetBrains Mono).

#### 5. `_layouts/home.html` is orphaned

`home.html` lists `site.posts` under a "Writings" heading, but `index.md` is `layout: default`, not `layout: home`. The homepage never uses it. Either (a) switch `index.md` front matter to `layout: home` and move the bio into `home.html` before the post list, or (b) delete `home.html` and keep the home page bio‑only — if so, be explicit about it.

#### 6. Duplicate font loading

`head.html` requests **five** families totaling ~14 weights/styles from Google Fonts on every page. Two of them (Cormorant Garamond, Source Serif 4) are used only on the site title and heading cascade, and multiple weights per family may not all be used.

**Fix:** audit actual font-weight/style usage, then request only those. A single `<link>` tag with trimmed variants can cut Critical-Path CSS by a lot on first paint. Also set `font-display: swap` behavior is already implied by `&display=swap`, good.

#### 7. Profile image — oversize and no intrinsic dimensions

- `assets/images/profile.jpg` is **1000×1155**, **~327 KB**, rendered at **110×110** (`index.md`, `.profile-photo`). Roughly 2–3 MP more than you'll ever show.
- The `<img>` has no `width`/`height` attributes, so the browser can't reserve space — expect a small CLS (cumulative layout shift) on load.
- JPEG at that display size should be ~15–25 KB at 220×220 (2× for HiDPI).

**Fix:** re-export at 220×220 (and/or 440×440 for `srcset`), compress to WebP/AVIF + JPEG fallback, add `width="110" height="110"` and `loading="eager"` (or `loading="lazy"` if moved below the fold). One-liner with `cwebp` / Squoosh / an `Image::Optim` pass.

#### 8. No favicon linked

`assets/images/favicon.svg` exists (good — SVG favicon, 4.4 KB). There is **no** `<link rel="icon" …>` in `head.html`, so browsers fall back to the default or a 404.

**Fix:** add to `head.html`:

```html
<link rel="icon" type="image/svg+xml" href="{{ '/assets/images/favicon.svg' | relative_url }}">
<link rel="apple-touch-icon" href="{{ '/assets/images/favicon.svg' | relative_url }}">
```

And consider shipping a 512×512 PNG fallback for older browsers.

#### 9. Missing `og:image` / `twitter:image`

`jekyll-seo-tag` emits OG + Twitter tags (verified in `_site/index.html:8-17`), but because no `site.image` or page‑level `image:` front matter is set, preview cards will have no image. `twitter:card` is `summary` (small card) — fine for text; upgrade to `summary_large_image` if/when you add a banner.

**Fix:** either drop a 1200×630 share image at `/assets/images/og.png` and add `image: /assets/images/og.png` to `_config.yml`, or set it per-post in front matter. See https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md.

### P2 — Nice to have

#### 10. SEO plugins

`_config.yml` declares only `jekyll-feed`. `jekyll-seo-tag` and `jekyll-sitemap` are auto-loaded by GitHub Pages even without being declared — that's why SEO tags render — but for parity with local builds, declare them explicitly:

```yaml
plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap
```

`jekyll-sitemap` adds `/sitemap.xml` which helps discoverability, and `robots.txt` can then point to it.

#### 11. `Gemfile.lock` listed in `.gitignore`

Gemfile.lock is in `.gitignore` (line 6). For a site that isn't a library, you usually *want* to commit the lockfile so builds are reproducible. GH Pages itself builds against its own pinned set, but your local `bundle exec jekyll serve` benefits from a pinned lockfile. Low priority.

#### 12. `.DS_Store` files

`.DS_Store` is listed in `.gitignore`, but several exist on disk (`_layouts/.DS_Store`, `_sass/.DS_Store`, `assets/.DS_Store`, `assets/images/.DS_Store`, `_posts/.DS_Store`, root `.DS_Store`). Not in the repo, but clutters local views. Consider a global `.gitignore_global` and `git config --global core.excludesfile`.

#### 13. Conditional MathJax loading

MathJax CHTML is loaded on *every* page (~1 MB, async but still bytes). For pages with no math, skip it. Example — in `head.html`:

```liquid
{%- if page.math or layout.math or site.math_everywhere -%}
  <script> MathJax = { ... }; </script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js" async></script>
{%- endif -%}
```

Then opt in with `math: true` on posts that need it. You already use `math: true` in your template — wire it up.

#### 14. `_posts/template/…` and similar meta files

As noted in (1), move author-facing templates out of `_posts/`.

#### 15. 404 page inline styles

`404.html` uses an inline `<style>` block. Works, but you already ship `main.css` — promote those rules there and give the 404 container a class.

#### 16. Small SCSS hygiene

- You redeclare `.post-meta` twice with a `font-style: italic;` override. Intentional? The `.post-meta` on lists is `font-style: normal`, on post pages it's italic. Fine, just note that `archive.html` and `posts.html` render dates inside `.post-meta` and will inherit the italic rule too — visually confirm this is what you want.
- `!important` is used ~10 times on `.site-nav` to defeat minima defaults. Not a crime, but if you drop minima-the-theme and just keep the reset, you can remove most of these.

### P3 — Low/optional

- 17. `.obsidian/` tracked inside `_posts/` — add `.obsidian/` to `.gitignore` and `git rm -r --cached _posts/.obsidian`.
- 18. `gem "jekyll", "~> 4.4.1"` in `Gemfile` is commented out while `github-pages ~> 232` is active. Correct for GH Pages. No action.
- 19. The external-links JS runs unconditionally on every page — inexpensive, fine.
- 20. No `robots.txt`. Add one pointing to sitemap (requires plugin #10).

---

## Part 2 — Design & visuals

### P0 — Must fix

#### A. Homepage gives a visitor nothing to read

`index.md` + `home-intro` render: avatar, one paragraph, stop. No post teasers, no "latest writing," no "start here." Even stripped-down personal sites (e.g. Matt Might, Simon Willison, Andy Matuschak) surface *something* readable-in-one-click from the index. You already have `_layouts/home.html` that would list posts — it just isn't wired up (see code finding #5).

**Proposal options, in order of my preference:**

1. **Two-column / single-column hybrid:** keep the bio at the top, then add "Recent writings" listing the latest 5 posts with date + title + 1-line excerpt. This is the classic research blog pattern. Requires fixing code finding #1 first, obviously.
2. **"Now" + "Recent":** bio paragraph → short "Currently working on …" paragraph → 5 most recent posts.
3. **Bio-only landing (what you have today):** fine for a personality site, but then collapse the nav's "Writings" and "Archive" into a single prominent CTA on the homepage.

I'd go with (1). It's what the layout already wants to do.

#### B. Contrast failures on the accent color

Measured (WCAG 2.1 relative luminance):

| Foreground | Background | Ratio | Usage | AA? |
|---|---|---:|---|---|
| `#4D403A` cacao | `#faf8f5` pearl | **9.39:1** | body text | ✅ |
| `#8B5A2B` sienna | `#faf8f5` pearl | 5.51:1 | site title | ✅ |
| `#A3968D` taupe | `#faf8f5` pearl | **2.71:1** | tagline, post-meta, home-intro body, footer links, year/category headings, post excerpts, blockquotes | ❌ fails |
| `#DFDACF` khaki | `#faf8f5` pearl | 1.31:1 | default link underline | ❌ near-invisible |
| `#F2EFE9` code-bg | `#faf8f5` page-bg | 1.08:1 | code blocks vs page | — (not text, but code blocks don't visibly differentiate) |
| `#8B8178` comment | `#F2EFE9` code-bg | 3.32:1 | code comments | ⚠️ passes only as "large text" |
| `#4A7C4E` string | `#F2EFE9` code-bg | 4.27:1 | code strings | ⚠️ passes only as "large text" |

That `$grey-color` is the most-used secondary color on the site, and it's below AA for normal text. The `home-intro p` sets body copy to `$grey-color` — the one paragraph on your landing page is the worst offender.

**Fix:** darken `$grey-color` to roughly `#857A72` (≈ 4.5:1) or `#776D65` (≈ 5.5:1). A simple one-line variable change fixes the cascade for tagline/meta/footer/headings/excerpts in one shot.

**Also:** bump the link underline to `$grey-color` rather than `$grey-color-light`. Right now default-state links are nearly underlineless; hover is the first time readers see the underline is even there.

**Also:** code background (`#F2EFE9`) and page background (`#faf8f5`) are 1.08:1 apart. Either darken the code bg (e.g. `#EFEADC` or `#EDE5D4`) or add a left border rail to pre blocks. Your inline `code` already has a 1px border so it's fine; block `pre` depends mostly on that border too.

### P1 — Visual hierarchy and typography

#### C. The site is uniformly quiet

Post listing page headings are *smaller* than body text:

- Year heading on Archive: `0.8rem` (≈ 13.6 px)
- Category heading on Writings: `0.85rem` (≈ 14.5 px)
- Body copy: `17 px`
- Post-list link title: `1.05rem` (≈ 17.9 px)

By convention, group headers should be heavier or larger than body text so a reader scanning a long archive can find year/category anchors. Right now the year 2026 is less prominent than the post titles under it, which means the "by year" affordance stops working visually.

**Fix:** bump year/category headings to `1rem`/`1.1rem`, bold, and/or increase letter-spacing while keeping small caps. Or make them large serif markers (e.g. `1.5rem` Cormorant Garamond).

#### D. Too many fonts

Active stack (head.html): Cormorant Garamond, Source Serif 4, IBM Plex Serif, IBM Plex Sans, JetBrains Mono. Dead stack (custom-head.html): DM Serif Display, Fraunces. Main.scss also names Merriweather/Playfair via dead variables.

Five running families are a lot for a minimalist site. Consider:

- **Display / title:** Cormorant Garamond (or Source Serif 4 Display) — one choice.
- **Body / heading:** IBM Plex Serif — one family, several weights.
- **UI:** IBM Plex Sans — already loaded.
- **Code:** JetBrains Mono.

That's four, down from five-to-seven, and they read as a coherent system.

#### E. Site header eats the fold

`.site-header` has `padding: 4rem 0 3rem`, plus `3rem` site title, small tagline, and nav line. On a 900×600 laptop display the header takes ~50% of the first screen. Readers haven't seen a single word of content before scrolling. The elegant centered header is nice, but trim:

- `padding: 2.5rem 0 2rem` on desktop,
- `site-title` to `2.25–2.5rem`,
- add a 1-line "most recent post" peek immediately below the nav so the fold carries content.

#### F. Centered body text on `.page-content.centered`

`_layouts/page.html` wraps content in `.centered`, which does `text-align: center` on all child `<p>`. Works for the bio intro (short), fails for Projects/About pages once you add paragraphs. Centered paragraphs are hard to read because each line starts at an unpredictable x-offset.

**Fix:** scope `.centered` to only center the first block (or to the `.home-intro`/`.about-header`). Long-form page body should be left-aligned.

### P2 — Polish

- **G. Visited links are invisible.** `a:visited { color: $text-color }` — identical to default. For a long-form reading site, distinct visited-link styling (e.g., `$grey-color`) helps readers track what they've read.
- **H. Mobile nav at 0.75 rem (12 px).** Tap targets ~12 px tall plus 0.4rem vertical padding. Apple/Google recommend 44×44. Bump to 0.85rem and pad to 0.7rem vertical.
- **I. `.post-taxonomies` badges center under titles — looks fine, but test long category names (they'll wrap).**
- **J. `.home h3` is 1.5rem.** That's the home listing heading — only visible once you fix A and wire up `home.html`. Watch out that it doesn't out-shout the `h2.post-list-heading` above it (currently 0.85rem). After wiring, rebalance.
- **K. The `· · ·` hr ornament is lovely** — consider an equivalent treatment between posts on listing pages to give the archive a bit of rhythm.
- **L. No dark mode.** Given a serif-heavy reading site, this is a common ask. `prefers-color-scheme: dark` with a second palette (swap `$background-color`, `$text-color`, `$code-bg-color`) is a ~40-line addition.
- **M. Code copy button.** For a math/compsci blog, a "copy" button on `pre` blocks is a small affordance readers appreciate. Stimulus/vanilla JS, ~30 lines.
- **N. Reading time / word count on post.html.** One Liquid expression: `{{ content | number_of_words | divided_by: 200 }} min read`.

### P3 — Optional

- **O. Category bg uses sienna `#8B5A2B`** — readable on pearl at 5.51:1, good. On mobile the category pill can look heavy next to the small post title; consider a thinner outlined variant.
- **P. Blockquote uses `$grey-color` text** — fails contrast as noted (B). Fix with the global grey darkening.
- **Q. Horizontal rule ornament `content: "· · ·"`** — relies on font rendering the middle-dot character. On Windows Chrome with the primary font missing, fallback fonts may render it differently. Not a bug, just trivia.

---

## Part 3 — What I'd do first (concrete roadmap)

### ~30 min: unblock the blog

1. Rename `_posts/2026-02-28.md` to a valid slug, write/migrate real content into it.
2. Migrate `_posts/Numerical Analysis/1. Numerical Analysis.md` to `_posts/2026-03-04-numerical-analysis.md` with proper front matter (`title`, `date`, `categories: [Numerical Analysis]`, `math: true`).
3. Move `_posts/template/` to `_templates/` (or `_drafts/`) and add `exclude: [_templates]` if you choose the former.
4. `git rm -r --cached _posts/.obsidian` and add `.obsidian/` to `.gitignore`.

### ~1 hr: visual/accessibility pass

5. Change `.site-title` from `<h1>` to `<p>` in `_includes/header.html`; adjust `main.scss` selectors.
6. Darken `$grey-color` to `#857A72` (fixes the AA failures globally).
7. Bump default link underline to `$grey-color` instead of `$grey-color-light`.
8. Add `<link rel="icon">` for `favicon.svg` in `head.html`.
9. Re-export `profile.jpg` at 220×220 (~25 KB).
10. Add `og:image` via `site.image` in `_config.yml`.

### ~30 min: cleanup

11. `git rm -r _sass/minima/` and `_includes/custom-head.html`.
12. Either wire `index.md` to `layout: home` or delete `_layouts/home.html`.
13. Declare plugins (`jekyll-seo-tag`, `jekyll-sitemap`) explicitly in `_config.yml`.

### Later / when you care

- Responsive font trimming (limit weights).
- Conditional MathJax.
- Dark mode.
- Reading time.

---

## Appendix — files I read

- `_config.yml`
- `_includes/{head,custom-head,header,footer}.html`
- `_layouts/{home,page,post,posts,archive}.html`
- `assets/main.scss`, `_sass/minima/{custom-styles,custom-variables}.scss`
- `index.md`, `posts.md`, `archive.md`, `projects.md`, `404.html`
- `_posts/2026-02-28.md`, `_posts/Numerical Analysis/1. Numerical Analysis.md`, `_posts/template/{{date}}-title.md`
- `Gemfile`, `.gitignore`, `CNAME`
- Built output: `_site/index.html`, `_site/posts/index.html`, `_site/archive/index.html`

Live-site items that should be re-verified against the production build (marked **LIVE** in text): (a) whether GH Pages's SEO plugin emits the same tags as the local build, (b) Core Web Vitals / LCP of profile image in the wild, (c) whether the favicon absence causes a 404 in production logs.
