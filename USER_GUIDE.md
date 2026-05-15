# Site User Guide

A task-oriented reference for maintaining **eigenjs.com** — the Jekyll site at `eigenjs7/eigenjs7.github.io`. Written for you to return to after a few months away. It doesn't explain Jekyll from scratch; it explains *this* site's conventions.

---

## 1. Where things live

```
blog/
├── _config.yml         Site-wide settings (title, tagline, author, collections, exclusions)
├── _includes/          Reusable HTML partials (head, header, footer, service-card, project-card)
├── _layouts/           Page templates (default, page, post, home, posts, archive, service)
├── _posts/             Blog posts. Filename: YYYY-MM-DD-slug.md
├── _drafts/            Unpublished drafts (rendered only with --drafts flag)
├── _services/          Tutoring services (one markdown file per service)
├── _projects/          Projects (cards link externally; no detail pages)
├── assets/
│   ├── main.scss       ALL custom styles — colors, fonts, spacing, card grid
│   └── images/         Profile photo, favicon, post images
├── index.md            Homepage (About)
├── posts.md            Writings list
├── archive.md          Archive by category
├── services.md         Services page (renders card grid + Consulting blurb)
├── projects.md         Projects page (renders card grid)
├── CNAME               Custom domain — don't edit unless changing domains
├── Gemfile             Ruby dependencies
├── AUDIT.md            Dev-facing audit (excluded from build)
└── USER_GUIDE.md       This file (excluded from build)
```

Anything starting with `_` is a Jekyll-special directory. The `_site/` folder (generated) is the built output — never commit it.

---

## 2. Preview locally

```bash
cd /path/to/blog
bundle install           # first time only, installs Ruby gems
bundle exec jekyll serve
```

Opens at **http://localhost:4000/**. Rebuilds on every file save. Two exceptions:

- `_config.yml` changes do NOT hot-reload — stop and restart `jekyll serve`.
- SCSS changes sometimes miss the first build — force a rebuild with Cmd+S on any `.md` file.

To preview drafts in `_drafts/`:

```bash
bundle exec jekyll serve --drafts
```

---

## 3. Publish changes

`main` branch → GitHub Pages → live at eigenjs.com in 1–2 minutes.

```bash
git add <specific files>          # prefer specific paths over git add -A
git commit -m "Short imperative subject, under 70 chars"
git push origin main
```

There is **no staging environment**. If a Liquid or YAML error lands on main, the Pages build fails on GitHub and the old version stays live. Always check the **Actions** tab after a push.

---

## 4. Common tasks

### Add a blog post

Create `_posts/YYYY-MM-DD-my-slug.md`:

```yaml
---
title: "My Post Title"
date: 2026-04-20
categories: [research]
tags: [wolfram, notation]
math: true        # ONLY if the post uses LaTeX — omit otherwise
---

Your markdown content here. Inline math: $E=mc^2$. Display math:

$$
\int_0^\infty e^{-x^2}\,dx = \tfrac{\sqrt{\pi}}{2}
$$
```

**Notes:**

- `math: true` lazy-loads MathJax (~1MB). Omit on posts with no math to keep them fast.
- Filename date must be `YYYY-MM-DD`. Posts dated in the future don't publish unless you pass `--future` to `jekyll serve`.
- `categories` show up on the archive page; `tags` are shown on the post.

### Work on a draft without publishing

Put the file in `_drafts/` (no date prefix needed) instead of `_posts/`. It only renders with `jekyll serve --drafts`. Move into `_posts/` and rename to `YYYY-MM-DD-slug.md` when ready.

### Edit the tagline, site title, or contact links

Everything lives in `_config.yml`:

```yaml
title: Junseo Lee's Research Blog              # header title
description: "Exploring mathematics and computation"   # tagline (also used for SEO + share cards)
email: junseo.lee.1@stonybrook.edu
github_username: eigenjs7
linkedin_username: pauljunseo
author: Junseo Lee
```

Restart `jekyll serve` after editing this file.

### Add a service

Create `_services/my-service.md`:

```yaml
---
layout: service
title: "Service Name"
summary: "One-sentence description shown on the card."
metadata: "Tag · Tag · Tag"
order: 8
permalink: /services/my-service/
price: "30,000 KRW per 1-hour session"
register_url: "https://forms.gle/..."
glyph: "∑"
instructor: "Junseo Lee"
---

## Learning Outcomes
- ...

## Curriculum
- ...

## Materials
- ...

## Policies
- ...

## Pricing

{{ page.price }}

## Instructor

Short bio paragraph.
```

**Field conventions:**

- `summary` — single sentence, no trailing period (matches existing services).
- `metadata` — middot-separated tags (`·` is U+00B7), no trailing period.
- `order` — numeric. Lower numbers sort first on the services page. Gaps are fine (use 10, 20, 30 for easier re-ordering later).
- `glyph` — Unicode character used as placeholder in the card's left square. Existing glyphs: σ ∫ π. Pick anything that fits the "restrained academic" voice. Avoid characters that render as iOS emoji (see Troubleshooting).
- `register_url` — any URL. For "by inquiry" items, use `mailto:ljunseo1007@gmail.com` or a form link.

### Add a project

Create `_projects/my-project.md`:

```yaml
---
title: "Project Name"
summary: "One-sentence description."
metadata: "Tag · Tag"
order: 2
external_url: "https://..."
glyph: "⊛"
---
```

**Projects have no body content.** The collection is configured with `output: false` in `_config.yml`, so Jekyll doesn't build individual project pages. The card is the only interface, and clicking the card (or the ↗ arrow) opens `external_url` in a new tab.

### Replace a glyph with a real image

Add `image:` to the front matter:

```yaml
image: /assets/images/my-project.jpg
```

- Image must be **square** and at least **144×144 px** (for 72×72 display at 2× density).
- Put the file in `assets/images/`.
- When `image:` is set, it takes precedence over `glyph:` on the card. You can leave `glyph` in place as a fallback.

### Reorder cards

Change the `order:` numeric value in front matter. Lower comes first.

### Delete a service or project

Delete the file in `_services/` or `_projects/`. Commit the deletion. The card disappears on next build.

### Add a new navigation link

Edit `_includes/header.html`. The nav is hand-written (not generated from pages), so you add a new `<a class="page-link">` element manually:

```html
<a class="page-link {% if page.url == '/notes/' %}active{% endif %}"
   href="{{ "/notes/" | relative_url }}">Notes</a>
```

Keep the nav under 6 items — it's sized for mobile fit.

---

## 5. Front matter cheatsheet

| Field            | Services     | Projects     | Posts     | What it does                                      |
| ---------------- | ------------ | ------------ | --------- | ------------------------------------------------- |
| `layout`         | `service`    | —            | `post`    | Page template                                     |
| `title`          | required     | required     | required  | Card title / page heading                         |
| `summary`        | required     | required     | —         | One-line card description                         |
| `metadata`       | required     | required     | —         | Middot-separated tags                             |
| `order`          | required     | required     | —         | Card sort position (ascending)                    |
| `permalink`      | required     | —            | —         | URL path                                          |
| `price`          | required     | —            | —         | Rendered on detail page                           |
| `register_url`   | required     | —            | —         | CTA button target                                 |
| `external_url`   | —            | required     | —         | Where the card links (opens new tab)              |
| `glyph`          | optional     | optional     | —         | Placeholder symbol                                |
| `image`          | optional     | optional     | —         | Overrides glyph if set                            |
| `instructor`     | optional     | —            | —         | Currently unused in layout, reserved              |
| `date`           | —            | —            | required  | Publication date (also in filename)               |
| `categories`     | —            | —            | optional  | Shown on archive; grouping                        |
| `tags`           | —            | —            | optional  | Shown on post                                     |
| `math`           | —            | —            | optional  | `true` to load MathJax                            |

---

## 6. Design changes

Everything visual lives in **`assets/main.scss`**. Two key zones:

**Top of the file — color and font variables:**

```scss
$background-color: #faf8f5;    // Pearl
$text-color:       #4D403A;    // Cacao — body text
$title-color:      #8B5A2B;    // Warm sienna — titles, CTAs, hover
$brand-color-light:#A3968D;    // Taupe — decorative only
$grey-color:       #776D65;    // Secondary text (passes WCAG AA on Pearl)
$grey-color-light: #DFDACF;    // Khaki — borders only, never text
$code-bg-color:    #EDE5D4;    // Parchment — code blocks

$base-font-family: "Source Serif 4", Georgia, serif;   // body
$heading-font:     "Source Serif 4", Georgia, serif;   // H1–H6
$title-font:       "adobe-garamond-pro", "EB Garamond", Garamond, Georgia, serif;  // display / site title (Adobe Fonts)
$sans-font:        "Source Sans 3", ..., sans-serif;    // tagline
$code-font:        "JetBrains Mono", ..., monospace;    // code
```

Changing `$title-color` for example ripples to: site title, section headings on archive, service detail-page titles, Register CTA background, card hover border. That's intentional — one knob moves the whole accent color.

**Card grid** — in the section labeled `CARD GRID — Services & Projects`. Controls:

- `.card` — border, padding, hover border color
- `.card-image` — square size (72×72 desktop, 56×56 mobile)
- `.card-glyph` — placeholder size and color
- `.card-title`, `.card-metadata`, `.card-summary`, `.card-arrow` — typography

**To add a new font family:**

1. Add the family to the Google Fonts URL in `_includes/head.html`.
2. Add a `$my-font` variable near the existing `$sans-font` in `main.scss`.
3. Apply it: `font-family: $my-font;`

---

## 7. Math support

- Enable per-post with `math: true` in the post's front matter.
- Uses **MathJax 3** (not KaTeX).
- Inline: `$...$` or `\(...\)`
- Display: `$$...$$` or `\[...\]`
- MathJax is configured in `_includes/head.html` — to change delimiters, edit there.

---

## 8. Troubleshooting

**Jekyll build fails with a YAML error.**
Usually front-matter indentation. YAML uses spaces (never tabs). Values containing colons need quoting: `title: "Python: A primer"`, not `title: Python: A primer`.

**Build fails with a Liquid error.**
Check brace balance in templates: every `{{` needs `}}`, every `{%` needs `%}`. Typos like `{% endif` (missing `%}`) are the usual culprit.

**Changes don't appear locally.**
Hard-refresh the browser (Cmd+Shift+R). If that doesn't work, stop and restart `jekyll serve` — `_config.yml` edits are the most common cause.

**Changes don't appear on eigenjs.com after pushing.**
Check the **Actions** tab on GitHub. A failed build keeps the old version live. Fix the error, commit, push again.

**A character renders as an iOS emoji instead of a text glyph.**
Some Unicode arrows (`↗`, `↘`, `↙`, `↖`) have emoji presentation on iOS. Two fixes:

- Append `&#xFE0E;` (the text variation selector) after the character in the HTML template.
- Or pick a character without emoji form: `→`, `⤴`, `⬈`, `↦` work as plain text on all platforms.

**Card shows no image and no glyph.**
Front matter is missing both `image:` and `glyph:`. Add one.

**A post dated today doesn't appear.**
Jekyll uses the timestamp strictly — if your post is dated at a future time today (e.g. `2026-04-20 18:00:00` and it's currently `14:00`), it won't publish. Either drop the time from the date, or pass `--future` to `jekyll serve`.

**Images look blurry.**
Store at 2× the display size. For a 700px-wide image slot, save a 1400px-wide source.

**"Services" page shows the old hand-typed list instead of cards.**
You're on an old commit or `services.md` wasn't saved with the card-grid markup. The file should have `{% for service in sorted_services %}...{% endfor %}` in it.

---

## 9. File-level "don't touch without care"

- **`_config.yml` → `collections:`** — changing `permalink` breaks existing bookmarks and inbound links.
- **`_layouts/default.html`** (comes from Minima gem, not in your repo by default) — if you override it, make sure SEO meta tags still load via `{%- seo -%}`.
- **`CNAME`** — controls the custom domain. Only edit if you're moving domains.
- **`.gitignore`** — `_site/` is ignored for a reason (it's generated). Don't commit the build output.

---

## 10. Quick command reference

```bash
# Preview
bundle exec jekyll serve                    # normal
bundle exec jekyll serve --drafts           # include _drafts/
bundle exec jekyll serve --future           # include future-dated posts
bundle exec jekyll build                    # build without serving

# Git flow
git status                                  # see what's changed
git add <path> <path>                       # stage specific files
git commit -m "message"                     # commit
git push origin main                        # publish (triggers Pages build)

# Sanity checks
git log --oneline -10                       # recent commits
git diff                                    # unstaged changes
git diff --staged                           # staged changes
```

---

Last updated: April 2026. If something in the site changes structurally (new collection, new layout convention), update this guide in the same commit so future-you has accurate docs.
